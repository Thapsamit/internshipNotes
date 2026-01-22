# Docker Installation Guide

This guide provides step-by-step instructions to install specific versions of Docker and Docker Compose on Ubuntu 20.04.

## Target Versions
- **Docker**: 20.10.21
- **Docker Compose**: 1.25.0

## Prerequisites
- Ubuntu 20.04 LTS server
- sudo privileges
- Internet connection

## Installation Steps

### 1. Remove Existing Docker Installations

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

### 2. Update System and Install Prerequisites

```bash
sudo apt-get update
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

### 3. Add Docker's Official GPG Key

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

### 4. Add Docker Repository

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 5. Update Package Index

```bash
sudo apt-get update
```

### 6. Install Specific Docker Version (20.10.21)

```bash
# List available versions to verify
apt-cache madison docker-ce

# Install specific version
sudo apt-get install -y docker-ce=5:20.10.21~3-0~ubuntu-focal docker-ce-cli=5:20.10.21~3-0~ubuntu-focal containerd.io
```

### 7. Hold Docker Packages (Prevent Auto-updates)

```bash
sudo apt-mark hold docker-ce docker-ce-cli containerd.io
```

### 8. Install Docker Compose Version 1.25.0

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

### 9. Create Symbolic Link (Optional)

```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

### 10. Start and Enable Docker Service

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

### 11. Add User to Docker Group (Optional)

```bash
sudo usermod -aG docker $USER
```

**Note**: After running this command, log out and log back in for the group changes to take effect.

### 12. Verify Installation

```bash
sudo docker --version
sudo docker-compose --version
```

## Expected Output

```
Docker version 20.10.21, build 20.10.21-0ubuntu1~20.04.2
docker-compose version 1.25.0, build unknown
```

## Troubleshooting

### Common Issues

1. **Permission Denied**: Make sure you're using `sudo` for installation commands.

2. **Package Not Found**: If the exact version isn't available, check available versions with:
   ```bash
   apt-cache madison docker-ce
   ```

3. **Repository Issues**: Ensure the GPG key and repository are correctly added.

4. **Service Not Starting**: Check Docker service status:
   ```bash
   sudo systemctl status docker
   ```

### Uninstall (if needed)

```bash
sudo apt-get purge docker-ce docker-ce-cli containerd.io
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
sudo rm /usr/local/bin/docker-compose
sudo rm /usr/bin/docker-compose
```

## Important Notes

- **Production Environment**: Always test in a staging environment first
- **Backup**: Create system backups before installation
- **Firewall**: Configure firewall rules for Docker if needed
- **Version Lock**: Packages are held to prevent automatic updates
- **Ubuntu Version**: Instructions are for Ubuntu 20.04; adjust for other versions

## Security Considerations

1. Keep Docker daemon secure
2. Use non-root user when possible
3. Regularly update security patches (while maintaining version compatibility)
4. Configure proper firewall rules
5. Use Docker secrets for sensitive data

## Support

For issues specific to these versions:
- Docker 20.10.21: Check Docker documentation archive
- Docker Compose 1.25.0: Refer to legacy compose documentation

## License

This installation guide is provided as-is for educational and deployment purposes.
