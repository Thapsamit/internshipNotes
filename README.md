
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

































