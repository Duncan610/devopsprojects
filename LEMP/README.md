# LEMP Stack Web Application Project

A hands-on project demonstrating a full **LEMP Stack** setup on **AWS EC2**, including **Linux, Nginx, MySQL, PHP**, and database-driven web pages. This project showcases creating a dynamic “To-Do List” web application powered by PHP and MySQL.

---

## **Table of Contents**
1. [Project Overview](#project-overview)
2. [Project Objectives](#project-objectives)
3. [Project Architecture](#project-architecture)
4. [Step-by-Step Implementation](#step-by-step-implementation)
    - [1. Launching the EC2 Instance](#1-launching-the-ec2-instance)
    - [2. Installing Nginx](#2-installing-nginx)
    - [3. Installing PHP](#3-installing-php)
    - [4. Configuring Nginx with PHP-FPM](#4-configuring-nginx-with-php-fpm)
    - [5. Testing the Web Server](#5-testing-the-web-server)
    - [6. Installing and Configuring MySQL](#6-installing-and-configuring-mysql)
    - [7. Creating Database and User](#7-creating-database-and-user)
    - [8. Creating the To-Do List Table](#8-creating-the-to-do-list-table)
    - [9. Creating PHP Pages](#9-creating-php-pages)
5. [Screenshots](#screenshots)
6. [Key Learnings](#key-learnings)
7. [Next Steps / Enhancements](#next-steps--enhancements)
8. [License](#license)

---

## **Project Overview**
This project demonstrates how to:
- Set up a **LEMP stack** on an AWS EC2 instance.
- Configure **Nginx to serve PHP** web pages.
- Create a **MySQL database** and connect it to PHP using **PDO**.
- Build a dynamic web page that displays a **To-Do List** from the database.

---

## **Project Objectives**
- Deploy a LEMP stack from scratch.
- Connect a PHP web application to a MySQL database.
- Practice secure user creation and database permissions.
- Display dynamic content on a web page using PHP.

---

## **Project Architecture**

Client Browser --> Nginx --> PHP-FPM --> MySQL Database


Linux: Ubuntu 24.04 LTS

Nginx: Web server handling HTTP requests.

MySQL: Database storing to-do list data.

PHP-FPM: PHP processor interfacing between Nginx and MySQL.

Step-by-Step Implementation
1. Launching the EC2 Instance

Launched an Ubuntu 24.04 LTS EC2 instance on AWS.

Configured security group to allow HTTP (80), HTTPS (443), and SSH (22) access.

Connected via SSH:
```
ssh -i "lemp-stack-key-pair.pem" ubuntu@<Public_IP>
```

2. Installing Nginx
```
sudo apt update
sudo apt install nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```
Tested server:
```
curl localhost
```
3. Installing PHP
```
sudo apt install php-fpm php-mysql
```
Verified installation:
```
php -v
```
4. Configuring Nginx with PHP-FPM

Created custom server block:
```
sudo nano /etc/nginx/sites-available/projectLEMP
```
```
server {
    listen 80;
    server_name projectLEMP www.projectLEMP;

    root /var/www/projectLEMP;
    index index.html index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```
Enabled site and removed default:
```
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
sudo unlink /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl restart nginx
```
5. Testing the Web Server

Created index.html:
```
sudo nano /var/www/projectLEMP/index.html
```
Hello LEMP from hostname <EC2_HOSTNAME> with public IP <EC2_PUBLIC_IP>

Accessed in browser: http://<Public_IP>

6. Installing and Configuring MySQL
```
sudo apt install mysql-server
sudo mysql_secure_installation
```
Started MySQL and logged in:
```
sudo mysql -u root -p
```
7. Creating Database and User
```
CREATE DATABASE example_database;
CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
GRANT ALL ON example_database.* TO 'example_user'@'%';
```
8. Creating the To-Do List Table
```
USE example_database;

CREATE TABLE todo_list (
    item_id INT AUTO_INCREMENT,
    content VARCHAR(255),
    PRIMARY KEY(item_id)
);

INSERT INTO todo_list (content) VALUES ("My first important item");
INSERT INTO todo_list (content) VALUES ("My second important item");
```

9. Creating PHP Pages

Created todo_list.php:
```
sudo nano /var/www/projectLEMP/todo_list.php

<?php
$user = "example_user";
$password = "PassWord.1";
$database = "exampKey Learnings
```
Full LEMP stack deployment on AWS.

Configured PHP-FPM with Nginx.

Created and managed MySQL users and permissions.

Connected PHP application to a database using PDO.

Debugged server issues and confirmed dynamic content delivery.

Created and managed MySQL users and permissions.

Connected PHP application to a database using PDO.

Debugged server issues and confirmed dynamic content delivery.
