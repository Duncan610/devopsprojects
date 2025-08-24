# 🚀 LEMP Stack Deployment on AWS EC2
## 📖 Project Overview

This project demonstrates the deployment of a LEMP stack (Linux, Nginx, MySQL, PHP) on an AWS EC2 Ubuntu instance. The goal is to understand provisioning, configuring, and securing a web server environment to host dynamic web applications.

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

## ⚙️ Steps Performed
Provisioning an EC2 Instance

Logged into the AWS Management Console.

Launched a new EC2 instance with:

Ubuntu Server 24.04 LTS (64-bit).

Instance type: t2.micro (Free Tier).

Configured key pair for SSH access (lemp-stack-key-pair.pem).

Created a Security Group allowing:

22 (SSH) → open by default on EC2 to access via SSH

80 (HTTP) → Default port that allows browsers to access web pages in the internet

## The LEMP STACK
Linux: Ubuntu 24.04 LTS

Nginx: Web server handling HTTP requests.

MySQL: Database storing to-do list data.

PHP-FPM: PHP processor interfacing between Nginx and MySQL.

## Step-by-Step Implementation
1. Launching the EC2 Instance

Launched an Ubuntu 24.04 LTS EC2 instance on AWS.

Configured security group to allow HTTP (80) and SSH (22) access.

Started MySQL and logged in:

Connecting to the Server via SSH
```
chmod 400 lemp-stack-key-pair.pem
ssh -i "lemp-stack-key-pair.pem" ubuntu@<Public-IP>
```

2. Installing Nginx

Updating the Server
```
sudo apt update
sudo apt upgrade -y
```
Install nginx
```
sudo apt install nginx -y
```
Verified installation:
```
sudo systemctl status nginx
```
🔍 Testing Locally with curl

We can test if Nginx is serving requests locally from the Ubuntu shell:
```
curl http://localhost:80
curl http://127.0.0.1:80
```
Both commands do the same thing — the difference is:

localhost uses a DNS name.

127.0.0.1 uses the loopback IP address.

In both cases, if Nginx is working, you will see an HTML response (raw text).

You can also confirm the server’s public IP address with:
```
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```
🌍 Testing via Browser (from the Internet)

Now open a browser and go to:
```
http://<Public-IP-Address>:80
```
👉 Port :80 is optional because browsers use it by default.

If everything is configured correctly, you should see the Nginx Welcome Page:

✅ This confirms the server is correctly installed and accessible both locally and over the Internet (0.0.0.0/0).

3. Installing and Securing MySQL Database Server

With the Nginx web server successfully running, the next step in building the LEMP stack is setting up a Database Management System (DBMS). We use MySQL.

🔹 Step 1: Install MySQL Server

Update package repositories and install MySQL:
```
sudo apt install mysql-server -y
```
Once complete, verify installation by logging in as the root administrative user:
```
sudo mysql
```

This command connects as root without a password since we’re using sudo.

🔹 Step 2: Secure MySQL Installation

MySQL ships with a helper script to remove insecure defaults and harden access:
```
sudo mysql_secure_installation
```
This script guides you through several important steps:

Set a password for the MySQL root account → We used:
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```

Configure Password Validation Plugin → We chose MEDIUM strength (at least 8 characters, includes upper/lowercase, numbers, special chars).

Apply Security Best Practices:
```
Remove anonymous users.

Disallow remote root login.

Remove test database.

Reload privilege tables.
```
👉 This ensures the server follows industry best practices for least privilege and secure authentication.

🔹 Step 3: Verify Root Access with Password

To test authentication after hardening, log in with the new password:
```
sudo mysql -p
```
This time, MySQL prompts for a password before granting access — confirming that root login is secure.

Exit the console when done:
```
exit;
```
4. Installing PHP

Now that Nginx is serving web pages and MySQL is securely managing data, the final piece of the LEMP stack is PHP, which will process server-side logic and generate dynamic web content.

Unlike Apache (which integrates PHP directly into each request), Nginx uses an external program to handle PHP. This design choice leads to better performance and scalability, but it requires additional configuration.

🔹 Step 1: Install PHP-FPM and PHP-MySQL

Run the following command to install PHP packages:
```
sudo apt install php-fpm php-mysql -y
```

php-fpm (FastCGI Process Manager) → Handles PHP requests separately from Nginx for improved performance.

php-mysql → Allows PHP applications to communicate with MySQL databases.

👉 The core PHP interpreter and necessary dependencies are installed automatically.

🔹 Step 2: Confirm Installation

Check the installed PHP version:
```
php -v
```

You should see output similar to:
```
PHP 7.4.x (cli) (built: ...)
```

This confirms PHP is installed and ready.

🔹 Step 3: Why This Setup Matters 🚀

Configuring Nginx with PHP-FPM instead of embedding PHP directly (like Apache does) demonstrates:

Understanding of different web server architectures → You know that Nginx doesn’t interpret PHP natively.

Performance optimization → Offloading PHP execution to php-fpm enables handling high-traffic workloads more efficiently.

Real-world readiness → Most modern web apps (WordPress, Laravel, Symfony, etc.) depend on php-fpm + php-mysql in production.

✅ With this step complete, we now have all three major components of the LEMP stack installed:

Nginx → Web server

MySQL → Database

PHP → Application logic

Next, we’ll configure Nginx to process PHP files, making the stack fully functional.

4. Configuring Nginx to Use PHP Processor

At this stage, Nginx (web server) and PHP-FPM (PHP FastCGI Process Manager) are installed, but they are not yet integrated. By default, Nginx cannot process PHP files directly — it must be configured to pass PHP requests to PHP-FPM.

This step demonstrates how to:

Configure server blocks in Nginx (equivalent to Apache virtual hosts).

Host multiple sites/domains on the same server.

Properly route .php requests to PHP-FPM for dynamic content generation.

🔹 Step 1: Create a Web Root Directory

Instead of using the default /var/www/html, create a dedicated directory for your project:
```
sudo mkdir /var/www/projectLEMP
sudo chown -R $USER:$USER /var/www/projectLEMP
```

✅ This ensures cleaner management and allows hosting multiple applications/domains in the future.

🔹 Step 2: Create a New Nginx Server Block

Create a new config file inside sites-available:
```
sudo nano /etc/nginx/sites-available/projectLEMP
```

Add the following configuration:
```
server {
    listen 80;
    server_name projectLEMP www.projectLEMP;

    root /var/www/projectLEMP;
    index index.html index.htm index.php;

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

🔹 Step 3: Explanation of Key Directives

listen 80 → Tells Nginx to listen for HTTP traffic on port 80.

server_name → Specifies the domain name or public IP that this block should respond to.

root → Directory containing the application files.

index → Defines which file (HTML/PHP) to serve first.

location / → Ensures valid files are served, otherwise returns 404.

location ~ .php$ → Passes PHP requests to PHP-FPM.

location ~ /.ht → Blocks access to .htaccess files (security best practice).

🔹 Step 4: Enable the Configuration

Link the configuration into the sites-enabled directory:
```
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```

Disable the default config (to avoid conflicts):
```
sudo unlink /etc/nginx/sites-enabled/default
```

🔹 Step 5: Test and Reload Nginx

Before applying changes, test the syntax:
```
sudo nginx -t
```

If successful, reload Nginx:
```
sudo systemctl reload nginx
```

🔹 Step 6: Create a Test Landing Page

Add a simple index.html file to confirm the configuration:
```
sudo echo 'Hello LEMP from hostname' \
$(TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" \
-H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` \
&& curl -H "X-aws-ec2-metadata-token: $TOKEN" \
-s http://169.254.169.254/latest/meta-data/public-hostname) \
'with public IP' \
$(TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" \
-H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` \
&& curl -H "X-aws-ec2-metadata-token: $TOKEN" \
-s http://169.254.169.254/latest/meta-data/public-ipv4) \
> /var/www/projectLEMP/index.html
```

🔹 Step 7: Verify in Browser

Open your web browser and test:

By Public IP:
```
http://<Public-IP>:80
```

By Public DNS:
```
http://<Public-DNS>:80
```

You should see your custom landing page showing the server’s hostname and public IP. 🎉

👉 Note: If both index.html and index.php exist, Nginx will serve index.html first by default. Remove/rename it later when adding PHP apps.

✅ At this point, your LEMP stack is fully functional — Nginx can now process PHP files with the help of PHP-FPM.

5. Testing PHP with Nginx

At this point, the LEMP stack (Linux, Nginx, MySQL, PHP) is fully installed and operational. The next step is to confirm that Nginx is correctly passing .php files to the PHP processor (PHP-FPM).

This ensures that the dynamic content generated by PHP can be served through Nginx.

🔹 Step 1: Create a PHP Test File

Navigate to your project’s document root and create a new file:
```
nano /var/www/projectLEMP/info.php
```

Insert the following PHP code:
```
<?php
phpinfo();
?>
```

🔹 Step 2: Access the PHP Test Page

Open your browser and visit your server using either:

By Public IP:

```
http://<Public-IP>/info.php
```

By Public DNS:
```
http://<Public-DNS>/info.php
```

✅ You should see a PHP information page (phpinfo output), which provides detailed information about:

Installed PHP version

PHP configuration settings

Loaded extensions (e.g., php-mysql)

Server environment variables

This confirms that Nginx is successfully processing PHP files via PHP-FPM.

🔹 Step 3: Clean Up (Security Best Practice)

⚠️ The info.php file exposes sensitive details about your server environment (such as versions and enabled modules). Leaving it accessible publicly is a security risk.

Once you’ve confirmed PHP is working, remove the file:
```
sudo rm /var/www/projectLEMP/info.php
```

👉 If needed in the future, you can always recreate it.

✅ With this test complete, your LEMP stack is fully functional:

Nginx serves web content.

MySQL stores and manages data.

PHP-FPM processes dynamic requests.

You are now ready to build and deploy dynamic PHP-based web applications on your server. 🚀

6. Retrieving Data from MySQL Database with PHP

With Nginx, MySQL, and PHP all running, the final step is to confirm that PHP can successfully interact with MySQL. We’ll build a simple “To-Do List” application that queries a MySQL database and displays data dynamically in a browser.

This validates the full end-to-end workflow of the LEMP stack:

Nginx → serves requests

PHP-FPM → processes application logic

MySQL → provides persistent storage

🔹 Step 1: Create a Database and User

First, log into MySQL as root:
```
sudo mysql
```

Create a new database:
```
CREATE DATABASE example_database;
```

Create a dedicated user with a secure password (⚠️ for demo purposes we’re using PassWord.1, but in production use a stronger secret):
```
CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```

Grant full privileges on the new database:
```
GRANT ALL ON example_database.* TO 'example_user'@'%';
```

Exit MySQL:
```
exit
```

🔹 Step 2: Test the New User

Log in as the new user:
```
mysql -u example_user -p
```

Check databases:
```
SHOW DATABASES;
```

✅ You should see:

example_database
information_schema

🔹 Step 3: Create a Table and Insert Data

Create a todo_list table:
```
CREATE TABLE example_database.todo_list (
  item_id INT AUTO_INCREMENT,
  content VARCHAR(255),
  PRIMARY KEY(item_id)
);
```

Insert a few rows:
```
INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
INSERT INTO example_database.todo_list (content) VALUES ("My second important item");
INSERT INTO example_database.todo_list (content) VALUES ("My third important item");
INSERT INTO example_database.todo_list (content) VALUES ("And one more thing");
```

Verify data:
```
SELECT * FROM example_database.todo_list;
```

Expected output:
```
item_id	content
1	My first important item
2	My second important item
3	My third important item
4	And one more thing
```
Exit MySQL:
```
exit
```
🔹 Step 4: Build the PHP Application

Create a new PHP file in your project directory:
```
nano /var/www/projectLEMP/todo_list.php
```

Paste the following code:
```
<?php
$user = "example_user";
$password = "PassWord.1";
$database = "example_database";
$table = "todo_list";

try {
    $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
    echo "<h2>TODO</h2><ol>";
    foreach($db->query("SELECT content FROM $table") as $row) {
        echo "<li>" . $row['content'] . "</li>";
    }
    echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
?>
```

Save and exit.

🔹 Step 5: Test in Browser

Navigate to your server in a browser:
```
http://<Public-IP>/todo_list.php
```

or
```
http://<Public-DNS>/todo_list.php
```

✅ You should see a clean list of the items inserted into the todo_list table, dynamically retrieved from MySQL.

🔹 What We Achieved

Created a secure MySQL database and user.

Built a test table (todo_list) and populated it with records.

Connected PHP to MySQL using PDO.

Displayed database records dynamically through Nginx.
