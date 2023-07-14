# Config file

Host my-ec2-instance
    HostName 123.45.67.89
    User ec2-user
    IdentityFile /path/to/my-key.pem


# commands 
### Update the system
```sql

sudo apt update
```

### install python in ec2  
``` sql
sudo yum update -y
sudo yum install -y python3
```


### setup postgresql in aws ec2 ubuntu instance

```
 $ sudo apt-get update
 $ sudo apt-get install postgresql postgresql-contrib
```

- PostgreSQL should now be installed on your EC2 instance. By default, PostgreSQL creates a user named "postgres" with administrative privileges. You can switch to the "postgres" user by running the following command:

- Access postgresql using below command
```sql
sudo -i -u postgres
```

how to know pm2 process is running on which port ?
- sudo lsof -i -P -n | grep LISTEN





