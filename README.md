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




## How to setup in PM2:-

1. Install PM2 in aws ec2 instance
```
npm install pm2 -g
```


## How to increase volume of aws ec2 

```shell
lsblk # to view all blocks of volume

```

```


use grow part

```shell
sudo growpart /dev/xvda 1
 
sudo resize2fs /dev/xvda1

```


## How to add new key/value pair and new pem file if we lost the existing pem file of aws ec2 instance?

- Step 1 - Create new key value pair from aws instance
- Step 2 - The above will generate new .pem file
- Step 3 - After getting pem file Go to a local terminal from your computer and write below command to generate public key material from .pem file
```bash
ssh-keygen -y -f /path/to/pem/file.pem
```
Ex - 
```bash
ssh-keygen -y -f C:\Users\thapl\Downloads\amit_turnkey.pem
```
- Step 4 - Now connect to EC2 intance from AWS console 
- Step 5 - Write below command to open authorized keys
```bash 
nano .ssh/authorized_keys

```
- Step 6 - The above will open nano editor where the public key material is writtern
- Step 7 - Replace the content with the content that you got from the local terminal in step 3
- Step 8 - connect with new .pem file through ssh or termius


### How to install mysql in aws ec2 ubuntu instance?
- Install mysql-server
```bash
sudo apt install mysql-server
```
- Check status of mysql
```bash
sudo systemctl status mysql
```
- How to change password of mysql in aws ec2 ubuntu instance
- Refer to this blog step by step
  https://linuxhint.com/change-mysql-root-password-ubuntu/

- or follow directly these commands
- Stop mysql service
```bash
sudo systemctl stop mysql.service
```
- Set mysql without checks
```bash
sudo systemctl set-environment MYSQLD_OPTS="--skip-networking --skip-grant-tables"
```
- Start mysql service again
```bash
sudo systemctl start mysql.service
```
- Go to mysql without pass

```bash
sudo mysql -u root
```
- Flush all privileges first
```bash
flush privileges;
```
- Use mysql db

```bash
use mysql

```
- Alter root password

```bash
ALTER USER  'root'@'localhost' IDENTIFIED BY 'the-new-password';

```

- Revert back sql to normal mode with pass

```bash
sudo systemctl unset-environment MYSQLD_OPTS
```

- Remove modified system configuration
```bash
sudo systemctl revert mysql

```
- Kill all mysql processes
```bash

sudo killall -u mysql
```

- Last step restart again mysql service
```bash
sudo systemctl restart mysql.service
```



```bash
sudo systemctl stop mysql.service

```

### How to open mysql in aws ec2 ubuntu
```bash
mysql -h localhost -u root -p
```


## How to upgrade node js version in aws ec2 ubuntu instance?

` Install nvm 
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```
- Activate nvm
```bash
. ~/.nvm/nvm.sh
```

- install node js version

```bash
nvm install --lts
``` 




### How to uninstall mysql from aws ec2 ubuntu instance?


- stop mysql service
```bash
sudo systemctl stop mysql

```
- run below command
```bash
sudo apt purge mysql-server mysql-client mysql-common mysql-server-core-* mysql-client-core-*

```
- Run cleanups

```bash
sudo rm -rf /etc/mysql /var/lib/mysql

sudo apt autoremove
sudo apt autoclean


```

### UPADTED How to set root password to allow password auth with mysql in aws ec2 

``` bash
use mysql 
```
```bash
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'mypassword';

```

```
FLUSH PRIVILEGES;
```

- Now if you login you need the new password to login into mysql





## how to get and put object directly to aws s3

```js
const AWS = require('aws-sdk');
const fs = require('fs');
const PDFDocument = require('pdf-lib');

const s3 = new AWS.S3({
  accessKeyId: 'YOUR_ACCESS_KEY_ID',
  secretAccessKey: 'YOUR_SECRET_ACCESS_KEY',
});

const imageToDoc = async (req, res) => {
  const pdfData = await s3.getObject({ Bucket: 'your-bucket-name', Key: 'output.pdf' }).promise();
  const pdfDoc = await PDFDocument.load(pdfData.Body);

  const imageFile = await pdfDoc.embedJpg(req.files.imageFile.data);
  const jpgDims = imageFile.scale(0.35);

  const page = pdfDoc.addPage();
  const marginFromTop = 50;
  page.drawImage(imageFile, {
    x: page.getWidth() / 2 - jpgDims.width / 2,
    y: page.getHeight() - jpgDims.height - marginFromTop,
    width: jpgDims.width,
    height: jpgDims.height,
  });

  // Serialize the PDFDocument to bytes (a Uint8Array)
  const modifiedPdfBytes = await pdfDoc.save();

  // Upload the modified PDF back to S3
  await s3.putObject({ Bucket: 'your-bucket-name', Key: 'output.pdf', Body: modifiedPdfBytes }).promise();

  return res.status(200).json({ data: 'base 64 image' });
};

// Make sure to handle errors appropriately and replace 'your-bucket-name' and AWS credentials with your actual values.


```

### How to move a folder to another folder 

```bash
mv source_folder ~/destination_folder/

```





