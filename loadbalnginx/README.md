# Load Balancer Solution with Nginx, Jenkins CI/CD & SSL/TLS

This project demonstrates a **production-style DevOps setup** that combines:

* Nginx as a **Load Balancer**
* Multiple backend **Web Servers**
* A shared **NFS Server** for persistent storage
* **Jenkins** for CI/CD automation
* **SSL/TLS (HTTPS)** using Let’s Encrypt
* Deployment on **AWS EC2** with proper **security groups and Elastic IPs**

The goal is to simulate a real-world web application infrastructure with automation, security, and scalability in mind.

---

## Architecture Overview

* **Jenkins Server** – CI/CD automation
* **Nginx Load Balancer** – Traffic distribution + SSL termination
* **Web Server 1 & 2** – Apache + PHP
* **NFS Server** – Shared `/var/www` storage
* **MySQL Server** – Application database

---

## Step 1: AWS Infrastructure Setup

We created multiple EC2 instances and configured security groups to allow only required traffic.

### Security Group Rules

**Web Servers**

![Web server inbound rules](screenshots/webserver1inboundrules.png)

**NFS Server**

![NFS inbound rules](screenshots/nfsinboundrules.png)

**Database Server**

![DB inbound rules](screenshots/dbserverinboundrules.png)

**Load Balancer**

![Load balancer inbound rules](screenshots/loadbalancerinboundrules.png)

**Jenkins Server**

![Jenkins inbound rules](screenshots/jenkinsinboundrules.png)

---

## Step 2: NFS Server Configuration

We configured an NFS server to provide shared storage across web servers.

![Create export directory & restart NFS](screenshots/installingcreatingdirexportdirrestartingnfsserver.png)

This allows all web servers to share the same application files.

---

## Step 3: Web Servers Setup (Apache, PHP & NFS Client)

### Install Apache, PHP and NFS Client

![Install PHP, MySQL & NFS client](screenshots/installphpmysqlandnfsclient.png)

### Mount NFS Persistently

![Mount and persist NFS on Web 1](screenshots/installmoutpersistmounttestphponweb1.png)

![Mount and persist NFS on Web 2](screenshots/mountpersistmounttestphponweb2.png)

### Apache Installation

![Install Apache on Web 2](screenshots/installapacheonweb2.png)

---

## Step 4: MySQL Server Setup

We installed and secured MySQL for backend data storage.

![Install MySQL Server](screenshots/installingmysqlserver.png)

![Secure MySQL](screenshots/securemysql.png)

---

## Step 5: Nginx Load Balancer Configuration

### Install Nginx

![Install Nginx on Load Balancer](screenshots/installnginxonloadbalancer.png)

### Configure Backend Resolution & Load Balancing

![Configure Nginx Load Balancer](screenshots/installnginxconfigurebackendresolutionandconfigureloadbal.png)

### Test Load Balancer

![Test Nginx](screenshots/testnginx.png)

---

## Step 6: Domain Name & Elastic IP

To ensure a static public IP, an **Elastic IP** was allocated and attached to the Nginx Load Balancer.

![Elastic IP allocated](screenshots/elasticipaddress.png)

![Elastic IP released](screenshots/releaseelasticip.png)

---

## Step 7: SSL/TLS with Let’s Encrypt

We secured the application using HTTPS.

![Install Certbot & Request Certificate](screenshots/installcertbotrequestcert.png)

This enables encrypted communication on port **443**.

---

## Step 8: Jenkins Server Setup

### Install Java

![Install Java on Jenkins Server](screenshots/installjavaonjenkinsserver.png)

### Install Jenkins

![Install Jenkins](screenshots/installjenkinsonserver.png)

### Start Jenkins

![Start Jenkins](screenshots/startjenkins.png)

### Retrieve Initial Admin Password

![Get Jenkins Password](screenshots/gettingjenkinspassword.png)

### Jenkins Ready Page

![Jenkins is Ready](screenshots/jenkinsisreadypage.png)

---

## Step 9: Jenkins Configuration

### General Jenkins Dashboard

![Jenkins Dashboard](screenshots/generalpageonjenkins.png)

### Install Publish Over SSH Plugin

![Install Publish Over SSH](screenshots/installpublishoverssh.png)

![Installed Publish Over SSH](screenshots/installedpublishoverssh.png)

### SSH into NFS from Jenkins Server

![SSH into NFS from Jenkins](screenshots/sshintonfsinjenkinsserver.png)

### Successful Jenkins–NFS Connection

![Success connecting Jenkins to NFS](screenshots/sucessatmanagejenkinsconnectwithnfs.png)

---

## Step 10: CI/CD Pipeline with GitHub

### GitHub Webhooks

![GitHub Hooks](screenshots/githubhooks.png)

### First Jenkins Build

![First Jenkins Build](screenshots/firstbuild.png)

### Jenkins Build View

![View Jenkins Build](screenshots/viewofjenkinsbuild.png)

### Active Jenkins Job

![Active Jenkins](screenshots/activejenkins.png)

---

## Step 11: Artifact Management

### Artifact Archiving

![Artifact Archiving](screenshots/artifactarchiving.png)

### Artifacts Stored on Jenkins Server

![Artifacts stored locally](screenshots/artifactsstoredlocallyonjenkinsserver.png)

---

## Step 12: Automated Deployment to NFS

### README Changes Trigger Jenkins Build

![Changes on README in Jenkins](screenshots/changesonreadmeatjenkinsbuild.png)

### Console Output

![Console Output](screenshots/consoleoutputonthereadmeoutputonjenkins.png)

### View Changes on NFS Server

![View README changes on NFS](screenshots/viewofchangesonreadmeonnfsserver.png)

---

## Final Outcome

* Load-balanced web application
* Shared storage using NFS
* CI/CD pipeline with Jenkins
* Secure HTTPS using SSL/TLS
* Automated deployments from GitHub

---

