# MySQL Client-Server Architecture on AWS

## 📖 Project Overview
This project demonstrates a basic **Client-Server Architecture** using **MySQL Database Management System (DBMS)** deployed on AWS EC2 instances.

- **Server A (mysql server):** Runs MySQL Server  
- **Server B (mysql client):** Runs MySQL Client  
- Communication established over **private IP** within the same **VPC & Subnet**  
- Configured **remote access** to MySQL server on port `3306`  
- Secured access by allowing inbound rules only from the client’s IP  

---

## ⚙️ Steps Implemented
1. Launched two Linux EC2 instances:  
   - `mysql server` (MySQL Server installed)  
   - `mysql client` (MySQL Client installed)  

2. Configured **MySQL for remote connections**:  
   - Edited `mysqld.cnf` to set `bind-address = 0.0.0.0`  
   - Created a remote MySQL user with proper privileges
   
3.✅ Output

Verified connection with:

SHOW DATABASES;

Successfully created and queried databases remotely.

🚀 Skills Demonstrated

AWS EC2 (VPC, Subnet, Security Groups)

MySQL Server & Client setup

Client-Server architecture in cloud environments

Linux server administration

3. Updated AWS **Security Group Rules**:  
   - Allowed inbound traffic on port `3306` only from client’s private IP  

4. Connected successfully from `mysql client` to `mysql server` using:  
   ```bash
   mysql -u remote_user -p -h <mysql-server-private-IP>
