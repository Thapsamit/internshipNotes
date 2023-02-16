# AWS NOTES :-

## How to setup react app in AWS EC2 ubuntu:-

- To host a React application in an AWS EC2 Ubuntu instance, you can follow these general steps:

1. Launch an EC2 instance: Log in to the AWS console, navigate to the EC2 dashboard, and launch an Ubuntu instance.

2. Connect to the instance: Once the instance is launched, connect to it using SSH.

3. Install Node.js: Node.js is required to run a React application. You can install Node.js on the EC2 instance using the following commands (from aws documentation)


```
sudo apt-get update
```

4. Install Git: Git is required to clone the repository that contains your React application. You can install Git using the following command:

```
sudo apt-get install git
```

5. Clone the repository: Use Git to clone the repository that contains your React application.

6. Install dependencies: Navigate to the cloned repository and install the required dependencies using the following command:

```
npm install
```

7. Build the application: Build the application using the following command:

```
npm run build
```


8. Install a web server: You need to install a web server to serve the application. You can use Apache or Nginx. Install Apache using the following command:

```
sudo apt-get install apache2
```

9. Copy the build files: Copy the build files to the Apache document root directory:
   - get the path to your react application using command
   ```
   >> pwd
   ```

```
sudo cp -r /path/to/react-app/build/* /var/www/html/
```

10. start apache 2 service


```
sudo service apache2 start
```


### Make sure add inbound rules and outbound rules 
- inbound rules to be setup as port 80 and listen to anywhere

11. Run the react app using


```
https:<ec2-ipaddress>:80
```




