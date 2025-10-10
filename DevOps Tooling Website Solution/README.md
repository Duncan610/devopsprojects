# 🛠️ Web Solution with WordPress — Tooling Project  

## 📘 Project Overview  
This project demonstrates the deployment of a **scalable web solution** using **multiple EC2 web servers**, a **centralized NFS server**, and **a remote MySQL database** on AWS.  
The setup ensures **consistent website content**, **centralized data management**, and **high availability**.

---

## 🧩 Architecture Overview  
![Architecture Overview](images/showingall5instances.png)

| Component | Description |
|------------|--------------|
| **NFS Server** | Central file storage shared with all web servers. |
| **Web Servers (Web1, Web2, Web3)** | Host Apache web server and pull shared website content from the NFS server. |
| **Database Server (MySQL)** | Stores website data and user credentials. |
| **EBS Volumes** | Extra storage attached to NFS and DB servers for persistence. |

---

## ☁️ AWS Setup

### 1️⃣ Create EC2 Instances  
Launched 5 EC2 instances — 1 Database, 1 NFS, and 3 Web Servers — all within the same VPC.  
![Creating Instances](images/createinstance.png)  
![Showing All Instances](images/showingall5instances.png)

### 2️⃣ Configure Security Groups  
Opened necessary inbound ports:
- **Web servers:** TCP 80 (HTTP)
- **NFS server:** TCP/UDP 2049
- **Database server:** TCP 3306 (MySQL)

![Security Group Rules](images/securitygroupinboundrules.png)
![Web Server Security Group](images/webserversecgroup.png)
![Database Security Group](images/dbsecuritygroup.png)
![Webserver 2 Rules](images/Webserver2inboundrules.png)
![Webserver 1 Rules](images/webserver1inboundrules.png)

---

## 💾 NFS Server Configuration  

### 3️⃣ Attach and Configure EBS Volumes  
Attached 3 EBS volumes and verified block devices:  
![Inspecting Block Devices](images/inspectingblockdevices.png)  
![Listing Block Devices](images/listingblockdevices.png)

Created partitions and logical volumes:
![Creating Physical Volumes](images/createphysicalvolumes.png)
![Create Volume Group](images/createvolumegroup.png)
![Create Logical Volumes](images/createlogicalvolumes.png)

Formatted the volumes and mounted them:
![Format with EXT4](images/formatwithext4.png)
![Format LVs with XFS](images/formatlvswithxfs.png)
![Mount LVs](images/mountlvs.png)
![DB Logical Volume Verification](images/verificationofdblogicalvolume.png)

Configured mount points:
![Create Mount Points](images/createmountpoints.png)
![Check Mounts](images/checkmounts.png)

---

### 4️⃣ Install and Configure NFS  
Installed NFS utilities and shared directories:
![Install NFS Server Package](images/installnfsserverpackage.png)
![Enable and Start NFS Server](images/enablestartandcheckstatusofnfsserver.png)
![Verification of Exports](images/verificationofexports.png)
![Check NFS Ports](images/checknfsports.png)

---

## 🌐 Web Servers Configuration  

### 5️⃣ Install Apache and Dependencies  
![Installing Apache](images/installingapache.png)
![Restart Apache](images/restartapache.png)

### 6️⃣ Mount NFS Shared Directory  
Mounted `/mnt/apps` from NFS server to each web server:  
![Create Mount Point and Mount NFS Export](images/createmountpointandmountnfsexportweb1.png)
![Make It Persistent Across Reboots](images/makeitpersistentacrossrebootsweb1.png)
![Verification of Mount](images/verificationofthemount.png)

---

## 🗄️ Database Server Configuration  

### 7️⃣ Install MySQL and Secure It  
![Install MySQL Server](images/installmysqlserver.png)
![Secure MySQL Installation](images/securemysqlinstallation.png)
![Enable and Start MySQL Server](images/enableandstartmysqlserver.png)

Created database and user:
![Create Database and User](images/createdatabaseanduser.png)
![Update DB Server Details](images/updatedbserverdetails.png)
![Allow Remote Connections](images/allowremoteconnection.png)

---

## 💻 Website Deployment  

### 8️⃣ Clone and Deploy Website Code  
Forked and cloned the tooling website from GitHub:  
![Cloning the Forked Repo](images/cloningtheforkedrepo.png)

Deployed website files to `/var/www/html`:  
![Configuration Test Output](images/configurationtestoutput.png)

### 9️⃣ Configure PHP Connection  
Updated `functions.php` to connect to the remote MySQL database:  
![Updating functions.php](images/updatingfunctions.phpconnectingtomysqlandverifying.png)

---

## 🔍 Verification  

Accessed the website on each web server’s public IP:  
![Web Server 1 Login Page](images/webserver1toolinglogin.png)
![Web Server 2 Login Page](images/webserver2loginpage.png)
![Web Server 3 Login Page](images/webserver3loginpage.png)


---

## 🧠 Cloud Concepts Simplified  

| Concept | Explanation | Analogy |
|----------|--------------|----------|
| **EBS Volume** | Extra storage attached to an EC2 instance for data persistence. | Like plugging in an external hard drive. |
| **Mounting** | Making an external drive accessible at a directory. | Like opening a folder from a USB drive on your laptop. |
| **NFS (Network File System)** | Lets multiple servers share and access the same files over a network. | Like having a shared Google Drive for your team. |
| **Apache Web Server** | Software that serves web pages to users’ browsers. | Like a waiter serving food (webpages) to customers (users). |

---

## 🚀 Outcome
✅ All three web servers displayed the same webpage.  
✅ Centralized NFS storage ensured consistency.  
✅ Remote database connection successfully configured.  

---

## 📈 Lessons Learned
- How to create and attach EBS volumes.  
- How to configure LVM for flexible storage management.  
- How to set up NFS for shared storage.  
- How to deploy and configure a website on multiple web servers.  
- How to connect web applications to a remote database.  

---
