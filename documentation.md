# LAMP STACK IMPLENTATION
## Connecting to EC2 terminal

```
cd ~/Downloads
```

## Change premissions for the private key file (.pem)

```
sudo chmod 0400 <private-key-name>.pem
```

## Connect to the instance by running

```
ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>
```
## Installing Apache and updateing firewall

```
#update a list of packages in package manager
sudo apt update

#run apache2 package installation
sudo apt install apache2
```
## verify that apache2 is running as a Service in our OS

```
sudo systemctl status apache2
```

## Verify accessibility locally

```
curl http://localhost:80
or
 curl http://127.0.0.1:80
```
## Test HTTPS servers remotely
```
http://<Public-IP-Address>:80
```
## Install and log into MYSQL

```
sudo apt install mysql-server
sudo mysql
```
## Connect to MYSQL server as root
```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 8.0.22-0ubuntu0.20.04.3 (Ubuntu)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```
## Set user and password
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```
## Exit MYSQL
```
mysql> exit
```
### Run a security script
```
sudo mysql_secure_installation
#Answer Y for yes, or anything else to continue without enabling

VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No:

#NEXT

There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary              file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1

#NEXT


Estimated strength of the password: 100 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y

```
## Test login and exit
```
sudo mysql -p

mysql> exit
```
## Install PHP and dependencies
```
sudo apt install php libapache2-mod-php php-mysql
```
## Creating a virtual host for your website using Apache 
```
sudo mkdir /var/www/projectlamp
```
## Assign ownership of the directory with your current system use
```
sudo chown -R $USER:$USER /var/www/projectlamp
```
## Create and open a new configuration file in Apache’s sites-available directory
```
sudo vi /etc/apache2/sites-available/projectlamp.conf

#this will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:

<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```
## Use the ls command to show the new file in the sites-available directory
```
sudo ls /etc/apache2/sites-available

#use a2ensite command to enable the new virtual host:

sudo a2ensite projectlamp

# disable the default website that comes installed with Apache.
sudo a2dissite 000-default

# make sure your configuration file doesn’t contain syntax errors, run:
sudo apache2ctl configtest

# sudo systemctl reload apache2
```
## Your new website is now active, Create an index.html file in that location so that we can test that the virtual host works as expected:
```
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' 
$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```
## open your website URL using IP address
```
http://<Public-IP-Address>:80
```
## Enable php on the server 
```
sudo vim /etc/apache2/mods-enabled/dir.conf

# Next

<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>

# Next, reload Apache so the changes take effect:
sudo systemctl reload apache2
```
## Create a new file named index.php inside your custom web root folder:
```
vim /var/www/projectlamp/index.php

# Next, This will open a blank file. Add the following text, which is valid PHP code, inside the file:
<?php
phpinfo();

# Finally
sudo rm /var/www/projectlamp/index.php
```














































###

