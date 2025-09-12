# 📚 Book Management App (Node.js + MongoDB on AWS EC2)

This project demonstrates how to set up a simple **Book Management Application** using **Node.js, Express, and MongoDB** hosted on an **AWS EC2 instance**.  
It supports adding, viewing, and deleting books via REST APIs.

---

## 🚀 Project Setup

### 1. Launch EC2 Instance
- Launch an **Ubuntu EC2 instance** in AWS.  
- Make sure to select a security group that allows inbound SSH and a custom **TCP port (3300)** for the app.  

![Launch EC2](screenshots/launchmeaninstance.png)

---

### 2. Install Dependencies

#### Update and install certificates
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y ca-certificates curl gnupg

