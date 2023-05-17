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
