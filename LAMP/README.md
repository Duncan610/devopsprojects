# 🚀 Setting Up a LAMP Stack on AWS EC2 (Ubuntu 24.04 LTS)

This guide provides step-by-step instructions for installing and configuring a LAMP stack (Linux, Apache, MySQL, PHP) on an AWS EC2 instance running Ubuntu 24.04 LTS. 
Upon completion, the setup will include a fully functional web server capable of hosting dynamic websites or web applications.

## 📦 What is LAMP?

The LAMP stack is a set of open-source technologies used for building and hosting websites:

Linux → Operating system

Apache → Web server

MySQL → Relational database

PHP → Programming language (can be swapped with Python or Perl)

## 0️⃣ Prerequisites – Launch & Prepare Your EC2 Instance

Launch an EC2 Instance

Go to AWS Management Console → EC2 → Launch Instance

Choose Ubuntu 24.04 LTS AMI

Select t2.micro (Free Tier eligible)

Choose a region close to you (e.g., eu-west-2a)

Create or use an existing key pair (.pem file) for SSH access

Configure Security Group to allow:

Port 22 (SSH)

Port 80 (HTTP)

Connect via SSH
```
ssh -i "your-key.pem" ubuntu@<EC2_PUBLIC_IP>
```
1️⃣ Update System Packages

Always start with updated packages:
```
sudo apt update && sudo apt upgrade -y
```
2️⃣ Install Apache

Apache will serve web pages to users.
```
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl status apache2
```

Test Apache:
Visit:
```
http://<EC2_PUBLIC_IP>
```

You should see the Apache2 Ubuntu Default Page.

3️⃣ Install MySQL

MySQL stores and manages application data.
```
sudo apt install mysql-server -y
sudo mysql
```

Set root password & enable password login:
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
FLUSH PRIVILEGES;
EXIT;
```

Login with password:
```
sudo mysql -p
```
4️⃣ Install PHP

PHP processes dynamic content.
```
sudo apt install php libapache2-mod-php php-mysql -y
php -v
```
5️⃣ Create a Virtual Host

Create project directory:
```bash
sudo mkdir /var/www/projectlamp
sudo chown -R $USER:$USER /var/www/projectlamp
```

Create Apache config:
```bash
sudo nano /etc/apache2/sites-available/projectlamp.conf
```

Paste:
```
<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Enable the site & disable default:
```
sudo a2ensite projectlamp
sudo a2dissite 000-default
sudo apache2ctl configtest
sudo systemctl reload apache2
```
6️⃣ Add Test Content

HTML Test Page:
```
sudo bash -c 'echo "Hello LAMP from hostname $(TOKEN=$(curl -X PUT \"http://169.254.169.254/latest/api/token\" -H \"X-aws-ec2-metadata-token-ttl-seconds: 21600\") && curl -H \"X-aws-ec2-metadata-token: $TOKEN\" -s http://169.254.169.254/latest/meta-data/public-hostname) with public IP $(TOKEN=$(curl -X PUT \"http://169.254.169.254/latest/api/token\" -H \"X-aws-ec2-metadata-token-ttl-seconds: 21600\") && curl -H \"X-aws-ec2-metadata-token: $TOKEN\" -s http://169.254.169.254/latest/meta-data/public-ipv4)" > /var/www/projectlamp/index.html'
```
Visit:
```
http://<EC2_PUBLIC_IP>
```
7️⃣ Enable PHP for the Site

Set index.php priority:
```
sudo nano /etc/apache2/mods-enabled/dir.conf
```
Make sure:
```
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

Reload Apache:
```
sudo systemctl reload apache2
```
8️⃣ Test PHP

Create PHP test file:
```
sudo nano /var/www/projectlamp/index.php
```

Add:
```
<?php
phpinfo();
?>
```

Visit:
```
http://<EC2_PUBLIC_IP>/index.php
```
⚠️ Security Tip: Remove this file after testing:
```
sudo rm /var/www/projectlamp/index.php
```
