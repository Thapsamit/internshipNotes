# PostgreSQL Logical Replication + WireGuard VPN Setup
## Power BI Access to Production Data (Safely)

---

## Architecture Overview

```
Prod EC2 (172.31.27.123)
├── www_beshak_backend     → Live app database (untouched)
└── powerbi_replica        → Read-only replica (Power BI connects here)
        ↑
        Logical Replication (same server, near real-time)

Team Laptops → WireGuard VPN → powerbi_replica:5432
```

**Why this is safe:**
- Port 5432 (PostgreSQL) is never exposed to the internet
- WireGuard encrypts all traffic between laptop and server
- `powerbi_user` is read-only — cannot modify any data
- `powerbi_replica` is isolated from the live app database
- No SSH access is given to any team member

---

## Part 1 — PostgreSQL Logical Replication Setup

### Step 1 — Check current WAL level
```bash
sudo -u postgres psql -c "SHOW wal_level;"
```
Expected output should be `logical`. If it says `replica` or `minimal`, proceed to Step 2.

---

### Step 2 — Find PostgreSQL config file location
```bash
sudo -u postgres psql -c "SHOW config_file;"
```
Typically located at `/etc/postgresql/12/main/postgresql.conf`

---

### Step 3 — Change WAL level to logical
```bash
sudo sed -i "s/#wal_level = replica/wal_level = logical/" /etc/postgresql/12/main/postgresql.conf
```

Verify the change was applied:
```bash
grep "wal_level" /etc/postgresql/12/main/postgresql.conf
```
Should show: `wal_level = logical`

> **⚠️ Important:** This requires a PostgreSQL restart (Step 4) which causes brief downtime (~10 seconds). Plan for off-peak hours on production.

---

### Step 4 — Restart PostgreSQL
```bash
sudo systemctl restart postgresql
```

---

### Step 5 — Verify WAL level is now logical
```bash
sudo -u postgres psql -c "SHOW wal_level;"
```
Should now show `logical`.

---

### Step 6 — Check replication slots and WAL senders
```bash
sudo -u postgres psql -c "SHOW max_replication_slots;"
sudo -u postgres psql -c "SHOW max_wal_senders;"
```
Both should be `5` or higher. If they are `0`, update `postgresql.conf`:
```bash
sudo nano /etc/postgresql/12/main/postgresql.conf
# Set:
# max_replication_slots = 5
# max_wal_senders = 5
# Then restart PostgreSQL
```

---

### Step 7 — Create the replica database
```bash
sudo -u postgres psql -c "CREATE DATABASE powerbi_replica;"
```

---

### Step 8 — Copy table structure to replica database
Copy schema only (no data) — logical replication handles data separately:
```bash
sudo -u postgres pg_dump -d www_beshak_backend -t marketplace_lead --schema-only | sudo -u postgres psql -d powerbi_replica
```

> **Note:** You may see foreign key errors like `relation does not exist`. This is expected and harmless — the table structure itself will be created correctly. Verify with:
```bash
sudo -u postgres psql -d powerbi_replica -c "\d marketplace_lead"
```

> **Adding more tables later:** Repeat this step for each new table, then follow Steps 9-11 for that table.

---

### Step 9 — Create a dedicated replication user
Never use the `postgres` superuser for replication. Create a limited user:
```bash
sudo -u postgres psql -c "CREATE USER replicator WITH REPLICATION PASSWORD 'your_strong_password';"
```

---

### Step 10 — Grant SELECT permission to replicator
```bash
sudo -u postgres psql -d www_beshak_backend -c "GRANT SELECT ON marketplace_lead TO replicator;"
```

> **Adding more tables later:** Run this grant for each new table you add to replication.

---

### Step 11 — Create the Publication on source database
A publication is a "broadcast channel" that defines which tables to replicate:
```bash
sudo -u postgres psql -d www_beshak_backend -c "CREATE PUBLICATION powerbi_pub FOR TABLE marketplace_lead;"
```

> **Adding more tables later:**
```bash
ALTER PUBLICATION powerbi_pub ADD TABLE new_table_name;
GRANT SELECT ON new_table_name TO replicator;
```

---

### Step 12 — Create the replication slot manually
This avoids hanging caused by long-running idle transactions from the app:
```bash
sudo -u postgres psql -d www_beshak_backend -c "SELECT pg_create_logical_replication_slot('powerbi_sub', 'pgoutput');"
```

> **Why manual slot creation:** If your app has idle database connections with open transactions, `CREATE SUBSCRIPTION` will hang indefinitely waiting for them to finish. Creating the slot separately avoids this.

> **If it still hangs**, find and terminate idle app connections first:
```bash
sudo -u postgres psql -d www_beshak_backend -c "SELECT pid, usename, state, now() - xact_start AS duration FROM pg_stat_activity ORDER BY xact_start NULLS LAST;"
sudo -u postgres psql -d www_beshak_backend -c "SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE usename = 'your_app_user' AND state = 'idle';"
```

---

### Step 13 — Create the Subscription on replica database
```bash
sudo -u postgres psql -d powerbi_replica -c "CREATE SUBSCRIPTION powerbi_sub CONNECTION 'host=<SERVER_PRIVATE_IP> port=5432 dbname=www_beshak_backend user=replicator password=your_strong_password' PUBLICATION powerbi_pub WITH (create_slot = false, slot_name = 'powerbi_sub');"
```

Replace `<SERVER_PRIVATE_IP>` with:
- **Same server setup (Prod replica on Prod):** Prod private IP e.g. `172.31.27.123`
- **Cross server setup (Prod replica on Dev):** Prod private IP e.g. `172.31.27.123`

PostgreSQL will automatically do an **initial full copy** of all existing rows, then stream all future changes in near real-time.

---

### Step 14 — Verify data copied successfully
```bash
sudo -u postgres psql -d powerbi_replica -c "SELECT COUNT(*) FROM marketplace_lead;"
```

Also check replication is live by verifying the subscription status:
```bash
sudo -u postgres psql -d powerbi_replica -c "SELECT * FROM pg_stat_subscription;"
```
Should show `srsubstate = r` (ready/streaming).

---

### Step 15 — Create read-only Power BI user
```bash
sudo -u postgres psql -d powerbi_replica -c "CREATE USER powerbi_user WITH PASSWORD 'your_strong_password';"
sudo -u postgres psql -d powerbi_replica -c "GRANT CONNECT ON DATABASE powerbi_replica TO powerbi_user;"
sudo -u postgres psql -d powerbi_replica -c "GRANT USAGE ON SCHEMA public TO powerbi_user;"
sudo -u postgres psql -d powerbi_replica -c "GRANT SELECT ON ALL TABLES IN SCHEMA public TO powerbi_user;"
```

> **Adding more tables later:** Run this to ensure new tables are also accessible:
```bash
sudo -u postgres psql -d powerbi_replica -c "GRANT SELECT ON ALL TABLES IN SCHEMA public TO powerbi_user;"
```

---

## Part 2 — WireGuard VPN Setup

### Step 16 — Install WireGuard on the server
```bash
sudo apt update && sudo apt install -y wireguard
```

Verify installation:
```bash
wg --version
```

---

### Step 17 — Generate server keypair (on server)
```bash
wg genkey | sudo tee /etc/wireguard/server_private.key | wg pubkey | sudo tee /etc/wireguard/server_public.key
```

View the keys:
```bash
sudo cat /etc/wireguard/server_private.key
sudo cat /etc/wireguard/server_public.key
```
> **⚠️ Keep the private key secret. Never share it.**

---

### Step 18 — Generate client keypair (on team member's laptop)
Install WireGuard on laptop first: https://www.wireguard.com/install/

**Mac/Linux:**
```bash
wg genkey > client_private.key
wg pubkey < client_private.key > client_public.key
cat client_private.key
cat client_public.key
```

**Windows (Command Prompt as Administrator):**
```cmd
cd C:\Program Files\WireGuard
wg genkey > client_private.key
wg pubkey < client_private.key > client_public.key
type client_private.key
type client_public.key
```

> **Each team member needs their own keypair.** Never share private keys. For each new team member, generate a new keypair and add them as a new `[Peer]` block in the server config (Step 19).

---

### Step 19 — Find the server's network interface
```bash
ip route | grep default
```
Note the interface name — typically `ens5` or `eth0`.

---

### Step 20 — Create WireGuard server config
```bash
sudo nano /etc/wireguard/wg0.conf
```

Paste the following (replace placeholders):
```ini
[Interface]
PrivateKey = <SERVER_PRIVATE_KEY>
Address = 10.0.0.1/24
ListenPort = 51820
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o ens5 -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o ens5 -j MASQUERADE

# Team Member 1
[Peer]
PublicKey = <CLIENT_PUBLIC_KEY>
AllowedIPs = 10.0.0.2/32

# Team Member 2 (add more peers as needed)
# [Peer]
# PublicKey = <CLIENT_2_PUBLIC_KEY>
# AllowedIPs = 10.0.0.3/32
```

Save with `Ctrl+X` → `Y` → `Enter`.

> **Adding more team members:** Add a new `[Peer]` block with their public key and assign the next IP (10.0.0.3, 10.0.0.4, etc.). Then run `sudo systemctl restart wg-quick@wg0`.

---

### Step 21 — Start WireGuard on server
```bash
sudo systemctl enable wg-quick@wg0
sudo systemctl start wg-quick@wg0
```

Verify it's running:
```bash
sudo systemctl status wg-quick@wg0
sudo wg show
```

---

### Step 22 — Open WireGuard port in AWS Security Group
1. Go to **AWS Console → EC2 → Instances**
2. Click your server instance → **Security** tab → click the Security Group
3. Click **Edit inbound rules** → **Add rule**:
   - **Type**: Custom UDP
   - **Port**: `51820`
   - **Source**: `0.0.0.0/0`
4. Click **Save rules**

> **Why `0.0.0.0/0` is safe for port 51820:** WireGuard silently drops all packets from unknown peers — it doesn't even respond. Only laptops with a valid private key matching a known public key can connect. IP restriction provides no additional security here since WireGuard's cryptography handles authentication.

---

### Step 23 — Create client config file (on laptop)
Create a file named `wg-client.conf` (ensure it's `.conf` not `.conf.txt`):

```ini
[Interface]
PrivateKey = <CLIENT_PRIVATE_KEY>
Address = 10.0.0.2/24
DNS = 8.8.8.8

[Peer]
PublicKey = <SERVER_PUBLIC_KEY>
Endpoint = <SERVER_PUBLIC_IP>:51820
AllowedIPs = <SERVER_PRIVATE_IP>/32
```

Replace:
- `<CLIENT_PRIVATE_KEY>` → your laptop's private key
- `<SERVER_PUBLIC_KEY>` → server's public key
- `<SERVER_PUBLIC_IP>` → server's public IP e.g. `65.1.62.253`
- `<SERVER_PRIVATE_IP>` → server's private IP e.g. `172.31.27.123`

> **Why `AllowedIPs = <SERVER_PRIVATE_IP>/32`:** Only traffic to the server's private IP goes through the VPN tunnel. All other internet traffic (browser, Slack, etc.) uses your normal connection as usual.

---

### Step 24 — Import and activate VPN on laptop
1. Open **WireGuard app**
2. Click **Add Tunnel** → **Import tunnel(s) from file**
3. Select `wg-client.conf`
4. Click **Activate**

Verify the tunnel is working:

**Windows (PowerShell):**
```powershell
Test-NetConnection -ComputerName <SERVER_PRIVATE_IP> -Port 5432
```

**Mac/Linux:**
```bash
nc -zv <SERVER_PRIVATE_IP> 5432
```

Should show `TcpTestSucceeded: True` or `succeeded`.

---

## Part 3 — Connect Power BI

1. Ensure **WireGuard VPN is activated** on your laptop
2. Open **Power BI Desktop** → **Get Data** → search **PostgreSQL** → **Connect**
3. Fill in:
   - **Server**: `<SERVER_PRIVATE_IP>` (e.g. `172.31.27.123`)
   - **Database**: `powerbi_replica`
4. Click **OK**
5. Enter credentials:
   - **Username**: `powerbi_user`
   - **Password**: `your_powerbi_user_password`
6. Click **Connect**
7. Select `marketplace_lead` table → **Load**

> **Note:** If prompted about encryption, you can disable it for internal connections. This is expected for self-hosted PostgreSQL without SSL certificates.

---

## Ongoing Maintenance

### Adding a new table to replication
```bash
# 1. Copy table structure to replica
sudo -u postgres pg_dump -d www_beshak_backend -t new_table_name --schema-only | sudo -u postgres psql -d powerbi_replica

# 2. Grant replicator access
sudo -u postgres psql -d www_beshak_backend -c "GRANT SELECT ON new_table_name TO replicator;"

# 3. Add table to publication
sudo -u postgres psql -d www_beshak_backend -c "ALTER PUBLICATION powerbi_pub ADD TABLE new_table_name;"

# 4. Grant powerbi_user access
sudo -u postgres psql -d powerbi_replica -c "GRANT SELECT ON ALL TABLES IN SCHEMA public TO powerbi_user;"
```

### Adding a new team member
```bash
# 1. Team member generates their keypair on their laptop
# 2. They share their client_public.key with you
# 3. Add new peer to server config
sudo nano /etc/wireguard/wg0.conf
# Add:
# [Peer]
# PublicKey = <NEW_MEMBER_CLIENT_PUBLIC_KEY>
# AllowedIPs = 10.0.0.X/32   ← use next available IP

# 4. Restart WireGuard
sudo systemctl restart wg-quick@wg0

# 5. Send them a wg-client.conf with their private key and IP (10.0.0.X)
```

### Revoking a team member's access
```bash
# Remove their [Peer] block from server config
sudo nano /etc/wireguard/wg0.conf

# Restart WireGuard
sudo systemctl restart wg-quick@wg0
```

### Monitoring replication health
```bash
# Check replication is active and streaming
sudo -u postgres psql -d powerbi_replica -c "SELECT * FROM pg_stat_subscription;"

# Check replication lag
sudo -u postgres psql -d www_beshak_backend -c "SELECT slot_name, confirmed_flush_lsn, pg_current_wal_lsn(), pg_current_wal_lsn() - confirmed_flush_lsn AS lag FROM pg_replication_slots;"

# Check replication slots
sudo -u postgres psql -d www_beshak_backend -c "SELECT * FROM pg_replication_slots;"
```

---

## Security Summary

| Layer | Protection |
|---|---|
| AWS Security Group | Only port 51820 (UDP) open to internet. Port 5432 hidden. |
| WireGuard encryption | All traffic encrypted with ChaCha20. Unknown peers silently dropped. |
| VPN authentication | Only laptops with valid keypair can connect. |
| PostgreSQL credentials | `powerbi_user` is read-only, only sees `powerbi_replica`. |
| Database isolation | `powerbi_replica` is completely separate from live app database. |
| `replicator` user | Only has SELECT on specific tables — not a superuser. |
