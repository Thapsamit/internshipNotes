
### How to setup nginx in AWS EC2 Instance ubuntu:-

- Install Nginx:
```
sudo apt update
sudo apt install nginx
```

- Start Nginx:
```
sudo systemctl start nginx

```

- Configure Nginx to proxy requests to your Node.js API:
  - (a) Create a new Nginx configuration file in the /etc/nginx/sites-available/ directory. For example:
  
```
sudo nano /etc/nginx/sites-available/myapi

```
  - (b) Add the following configuration to the file:
  ## If domain name is not there then write the ip address of the ec2 instance
  ## To run more than one environment we need to change the port numbers
  
```
server {
    listen 80;
    server_name myapi.example.com; # Replace with your domain name
    location / {
        proxy_pass http://localhost:3000; # Replace with your Node.js API URL
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}


```


- c. Create a symbolic link from the configuration file to the sites-enabled directory:

```
sudo ln -s /etc/nginx/sites-available/myapi /etc/nginx/sites-enabled/

```


- d. Test the Nginx configuration:

```
sudo nginx -t

```


- e. Restart Nginx:

```

sudo systemctl restart nginx

```



# How To setup Pm2 in AWS EC2 ubuntu instance

- Install PM2 globally on your EC2 instance:


```
sudo npm install -g pm2

```

- Navigate to your Node.js application directory:

```
cd /path/to/your/nodejs/application

```

- Start your Node.js application with PM2:

```
pm2 start index.js

```

Other commands can be 

```
pm2 restart process_id # to restart a process
pm2 ls  # to list down all the pm2 processes
pm2 stop all # tos top all the process
pm2 delete process_id # to delete all processes
```

## how to activate virtual env in aws ec2 ubuntu instance

```sql
source venv\bin\activate
```

























