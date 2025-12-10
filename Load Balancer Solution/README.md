# 🌀 Apache Load Balancer Solution on Ubuntu (DevOps Project)

This project demonstrates how to implement a **Load Balancer Solution using Apache on Ubuntu** as part of a **DevOps/Cloud Engineering workflow**.  
It showcases how multiple web servers can efficiently serve traffic through an Apache Load Balancer to improve availability, scalability, and fault tolerance.

---

## 🧱 Project Architecture

The setup consists of:
- **1 Load Balancer Server**
- **2 Web Servers (Web1 & Web2)**
- **1 NFS Server** (for shared storage)
- **1 Database Server** (for backend data management)

The Load Balancer routes incoming traffic between Web1 and Web2, while both web servers can access the shared NFS storage and communicate with the central database server.

### 🖥️ Architecture Diagram
```mermaid
graph TD
    User[🌍 Client/User] -->|HTTP Request| LB[⚙️ Apache Load Balancer]
    LB -->|Distributes Requests| Web1[🖥️ Web Server 1]
    LB -->|Distributes Requests| Web2[🖥️ Web Server 2]
    Web1 -->|Shared Storage| NFS[(📂 NFS Server)]
    Web2 -->|Shared Storage| NFS
    Web1 -->|DB Queries| DB[(🗄️ Database Server)]
    Web2 -->|DB Queries| DB

```

## ⚙️ Step 1: Launch EC2 Instances

Provision five Ubuntu EC2 instances on AWS:

Load Balancer

Web Server 1

Web Server 2

NFS Server

Database Server

✅ Make sure:

All instances are in the same VPC and availability zone (for simplicity)

Security groups are properly configured for inbound/outbound rules

Each server has a private IP for internal communication

<img src="screenshots/allfiveinstances.png" alt="All five instances launched">

## 🔒 Step 2: Configure Security Groups

Load Balancer Inbound Rules

<img src="screenshots/loadbalancerinboundrules.png" alt="Load Balancer inbound rules">

Web Server 1 Inbound Rules

<img src="screenshots/webserver1inboundrules.png" alt="Web Server 1 inbound rules">

Web Server 2 Inbound Rules

<img src="screenshots/webserver2inboundrules.png" alt="Web Server 2 inbound rules">

NFS Inbound Rules

<img src="screenshots/nfsinboundrules.png" alt="NFS inbound rules">

Database Server Inbound Rules

<img src="screenshots/dbinboundrules.png" alt="Database Server inbound rules">

## 🧰 Step 3: Install Apache on Web Servers

Web Server 1

```
sudo apt update -y
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl status apache2
```

<img src="screenshots/installingapacheonweb1.png" alt="Installing Apache on Web1">

<img src="screenshots/startingenablingandstatusofapacheonweb1.png" alt="Starting, enabling, and status of Apache on Web1">

Web Server 2

```
sudo apt update -y
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl status apache2
```

<img src="screenshots/installingapache2onweb2.png" alt="Installing Apache2 on Web2">

<img src="screenshots/startingenablingandstatusofapacheonweb2.png" alt="Starting, enabling, and status of Apache on Web2">

## 🌐 Step 4: Set Up HTML Content on Each Web Server

On Web1:

```
echo "<h1>This is Web Server 1</h1>" | sudo tee /var/www/html/index.html
```

<img src="screenshots/htmloutputconfirmingweb1responcelocally.png" alt="HTML output confirming Web1 response locally">

On Web2:

```
echo "<h1>This is Web Server 2</h1>" | sudo tee /var/www/html/index.html
```

<img src="screenshots/htmloutputonweb2confirminglocalresponse.png" alt="HTML output on Web2 confirming local response">

Confirm via browser or curl:

## 🧩 Step 5: Configure Local DNS Resolution (on Load Balancer)

Edit the /etc/hosts file on your Load Balancer:

```
sudo nano /etc/hosts
Add your private IP mappings:
text172.31.xx.xx Web1
172.31.yy.yy Web2
```
<img src="screenshots/webserverdnsrecords.png" alt="Web server DNS records">

Then test:

```
ping -c 2 Web1
```

```
ping -c 2 Web2
```

## ⚡ Step 6: Install and Configure Apache Load Balancer

Install Apache and required modules

```
sudo apt update -y
sudo apt install apache2 libxml2-dev -y
```

<img src="screenshots/installapache2onloadbal.png" alt="Install Apache2 on Load Balancer">

<img src="screenshots/installinglibxml2dev.png" alt="Installing libxml2-dev">

Enable proxy and load balancing modules

```
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod proxy_balancer
sudo a2enmod lbmethod_byrequests
sudo systemctl restart apache2
```

<img src="screenshots/modulesallowingapachetohandleproxyandloadbalancing.png" alt="Modules allowing Apache to handle proxy and load balancing">

<img src="screenshots/statusofapache2.png" alt="Status of Apache2">

## 🧱 Step 7: Configure Virtual Host on Load Balancer

Edit:

```
sudo nano /etc/apache2/sites-available/000-default.conf
```

Add:

```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName load-balancer
    DocumentRoot /var/www/html
    ProxyRequests Off
    ProxyPreserveHost On
    <Proxy "balancer://mycluster">
        BalancerMember http://Web1
        BalancerMember http://Web2
        ProxySet lbmethod=byrequests
    </Proxy>
    ProxyPass / balancer://mycluster/
    ProxyPassReverse / balancer://mycluster/
    <Location "/balancer-manager">
        SetHandler balancer-manager
        Require all granted
    </Location>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Test config and restart Apache:

```
sudo apache2ctl configtest
sudo systemctl restart apache2
```

<img src="screenshots/restartingapache.png" alt="Restarting Apache">

<img src="screenshots/loadbalapachewebpage.png" alt="Load Bal Apache webpage">

## 🔄 Step 8: Test Load Balancing

Run from the Load Balancer:

```
curl http://Web1
curl http://Web2
```

<img src="screenshots/curlcommandonloadbalancershowingweb1andweb2.png" alt="Curl command on Load Balancer showing Web1 and Web2">

<img src="screenshots/htmlcontentonloadbalaftercurlweb1.png" alt="HTML content on Load Bal after curl Web1">

<img src="screenshots/htmlcontentonloadbalaftercurlweb2.png" alt="HTML content on Load Bal after curl Web2">

Then test in browser using your public IP.

You should see alternating pages:

<img src="screenshots/loadbalancerscreenshotonbrowsershowingweb1.png" alt="Load Balancer screenshot on browser showing Web1">

<img src="screenshots/loadbalancerscreenshotonbrowsershowingweb2alternate.png" alt="Load Balancer screenshot on browser showing Web2 alternate">

## 🧮 Step 9: Enable Balancer Manager for Monitoring

```
sudo a2enmod status
sudo systemctl restart apache2
```

<img src="screenshots/enablingbalancermanagerformonitoringonlb.png" alt="Enabling Balancer Manager for monitoring on LB">

Access the manager at:

```
texthttp://<Load-Balancer-Public-IP>/balancer-manager
```

## 📂 Step 10: NFS and Database Integration

NFS Server: Provides a shared directory for web servers to store logs or assets.

Database Server: Centralized data store accessible by both web servers for dynamic web apps.

✅ Final Verification

Load Balancer alternates traffic between Web1 and Web2 seamlessly, confirming high availability and performance.

🧹 Cleanup

After completing the project:

Terminate EC2 instances to avoid extra AWS charges

Delete Elastic IPs and key pairs (if not needed)

Remove security groups no longer in use

## 🧾 Summary

Load Balancer - Routes requests between multiple web servers

Web Servers - Serve HTML content to users

NFS Server - Provides shared file storage

Database Server - Centralized data management

Apache Modules Used,"proxy, proxy_http, proxy_balancer, lbmethod_byrequests"

## 🚀 Outcome

✅ High availability

✅ Seamless traffic distribution

✅ Improved fault tolerance

✅ Modular DevOps setup ready for scaling
