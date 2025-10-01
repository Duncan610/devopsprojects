# Web Solution With WordPress  

This project demonstrates how to implement a **web solution using WordPress** with a **two-server architecture** on AWS.  

As a DevOps engineer, one of the most common solutions you will encounter is **PHP-based applications** such as WordPress, which is still one of the most widely used web content management systems in the world. WordPress is written in **PHP** and uses **MySQL** (or MariaDB) as its relational database management system (RDBMS).  

In this project, I deployed a **three-tier architecture** that mirrors how real-world web solutions are built:  

- **Presentation Layer (PL):** The client interface (browser or curl from a laptop/PC).  
- **Business Layer (BL):** The Web Server running Apache and WordPress.  
- **Data Layer (DAL):** The Database Server running MySQL.  

---

## 📌 Project Objectives  

The project was divided into **two main parts**:  

1. **Storage Subsystem Configuration**  
   - Configure and manage storage infrastructure for both the Web and Database servers using **EBS volumes** and **LVM (Logical Volume Manager)**.  
   - Work with partitions, physical volumes, volume groups, and logical volumes.  
   - Set up dedicated partitions for application files (`/var/www/html`) and log storage (`/var/log`).  

2. **WordPress Deployment**  
   - Install and configure **Apache, PHP, and WordPress** on the Web Server (Ubuntu).  
   - Install and configure **MySQL Server** on the Database Server.  
   - Connect WordPress (on the Web Server) to the MySQL Database (on the DB Server).  

---

## 🏗️ Technology Stack  

- **Cloud Provider:** AWS EC2  
- **Operating System:** Ubuntu Linux (instead of RedHat/CentOS)  
- **Storage Management:** EBS Volumes, gdisk, LVM  
- **Web Tier:** Apache, PHP, WordPress  
- **Database Tier:** MySQL Server  
- **Security:** AWS Security Groups, Firewall (UFW), MySQL user privileges  


## 📸 Step-by-Step Implementation

Below are the implementation steps with screenshots for reference.

### 1. Attach Volumes
Attach additional EBS volumes to the database server instance.  
![Attach Volumes](Images/attachvolumes.png)

### 2. Attach DB Volume
Attach the dedicated database storage volume.  
![Attach DB Volume](Images/attachdbvolume.png)

### 3. Inspect Block Devices
Check the attached volumes using `lsblk`.  
![Inspect Block Devices](Images/inspectingblockdevices.png)

### 4. Disk Partitioning
Partition the new volumes using `fdisk`.  
![Disk Partitioning](Images/diskpartitioning.png)

### 5. View of Partitions
Verify partitions with `lsblk`.  
![View Partitions](Images/viewofthepartitions.png)

### 6. Create Physical Volumes
Initialize partitions as physical volumes (PVs).  
![Create Physical Volumes](Images/createphysicalvolumes.png)

### 7. Create Volume Groups
Create volume groups (VGs) for better storage management.  
![Create Volume Groups](Images/createvolumegroups.png)

### 8. Create Logical Volumes
Create logical volumes (LVs) from the VGs.  
![Create Logical Volumes](Images/createlogicalvolumes.png)

### 9. DB Create PVs
PVs created specifically for the DB server.  
![DB Create PVs](Images/dbcreatepvs.png)

### 10. DB Create VG
Create VG for database files.  
![DB Create VG](Images/dbcreatevg.png)

### 11. Add Logical Volumes
Create logical volumes for logs and data.  
![Add Logical Volumes](Images/addlogicalvolumes.png)

### 12. DB Logical Volumes View
Check created logical volumes.  
![DB Logical Volumes View](Images/dblvmtoolsview.png)

### 13. DB LVM Tools
Install and check LVM tools.  
![DB LVM Tools](Images/dblvmtools.png)

### 14. DB lvlogs-lv
Dedicated LV created for DB logs.  
![DB lvlogs-lv](Images/db-lvlogs-lv.png)

### 15. Create Mount Points
Create mount points for DB and logs.  
![Create Mount Points](Images/createmountpoints.png)

### 16. Format with ext4
Format the logical volumes with ext4.  
![Format ext4](Images/formatwithext4.png)

### 17. Mount Logs
Mount the log partitions.  
![Mount Logs](Images/mountlogs.png)

### 18. Backup Logs
Backup existing log files before replacement.  
![Backup Logs](Images/backuplogs.png)

### 19. Restore Logs
Restore logs to new mounted partition.  
![Restore Logs](Images/restorelogs.png)

### 20. UUID of Device
Check UUID of partitions.  
![UUID of Device](Images/uuidofdevice.png)

### 21. UUID Configuration
Update `/etc/fstab` with UUID for persistence.  
![UUID Config](Images/uuid.png)

### 22. Verification of DB Logical Volume
Verify that the DB logical volumes are mounted correctly.  
![Verify DB LVs](Images/verificationofdblogicalvolume.png)

### 23. DB Recovery Files
Check and ensure DB recovery file locations.  
![DB Recovery Files](Images/dbrecoveryfiles.png)

### 24. DB Partitions
DB partitions successfully created.  
![DB Partitions](Images/dbpartitions.png)

### 25. DB Partitions View
Double-check DB partitions.  
![DB Partitions View](Images/dbpartitionsview.png)

### 26. Install MySQL Server
Install MySQL server on the DB instance.  
![Install MySQL](Images/installmysqlserver.png)

### 27. Start and Enable MySQL
Enable MySQL to start on boot.  
![Start and Enable MySQL](Images/startandenablemysql.png)

### 28. Update DB Server Details
Update DB server configuration files.  
![Update DB Details](Images/updatedbserverdetails.png)

### 29. Security Group for DB
Configure DB security group for restricted access.  
![DB Security Group](Images/dbsecgroup.png)

### 30. Connect WordPress to DB
Verify WordPress connects to the DB server.  
![Connect WordPress to DB](Images/connectwordpresstodb.png)

---

## ✅ Final Step
Both the Web Server and Database Server are now connected, and WordPress is running successfully.  
![Final Step](Images/finalstep.png)

---

## 🔑 Key Learnings
- How to use **AWS EC2 and EBS volumes** effectively.  
- How to configure **LVM for scalable storage management**.  
- How to separate **application and database tiers**.  
- Importance of **security groups and firewall rules** for controlled access.  


## 💡 Key Takeaways  

By completing this project, I:  
- Gained hands-on experience with **Linux storage management** (partitions, LVM, mounting, persistent volumes).  
- Deployed a **three-tier web architecture** separating the web and database layers.  
- Practiced configuring **AWS security groups and firewall rules** to allow controlled access between servers.  
- Learned to **troubleshoot connections** between WordPress and a remote MySQL database.  

This project builds a strong foundation for **DevOps and Cloud Engineering** skills by combining Linux administration, storage configuration, web deployment, and cloud resource management.  

