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

```
sudo apt update && sudo apt upgrade -y
sudo apt install -y ca-certificates curl gnupg
```

![Install Certificates](screenshots/installingcertificates.png)

### 3. Installing NodeJS and npm

#### Installing NodeJS

```
sudo apt install nodejs -y
node -v
```

![Install Node.js](screenshots/installnodejs.png)  

### Installing npm

```
sudo apt install npm -y
npm -v
```

![Installed npm](screenshots/installednpm.png)  

### 4. Install MongoDB

#### Install GnuPG

```
sudo apt-get install -y gnupg curl
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
```
![Install GnuPG](screenshots/installgnupg.png)  

### Install MongoDB
```
sudo apt-get install -y mongodb-org
```
![Installing MongoDB](screenshots/installingmongodb.png) 

### Starting, enabling, and checking the status of the server

```
sudo systemctl start mongodb
sudo systemctl enable mongodb
sudo systemctl status mongodb
```
![MongoDB Status](screenshots/mongodbstatus.png)  

### Installing and checking the status of Node Package Manager

```
sudo apt install npm -y
```
![Installed npm](screenshots/installednpm.png)  

### Installing dependencies

```
sudo npm install body-parser
```
![Install body-parser](screenshots/installbodyparser.png)  

### 5. Create Project Directory

```
mkdir Books
cd Books
```

#### Initialize npm

```
npm init -y
```
![Initialize npm](screenshots/initializenpm.png)  

### 6. Installing MOngoose

```
sudo npm install express mongoose
```
![Install mongoose](screenshots/installmongoose.png)  

#### Inside 'Books' folder, create a folder named apps

```
mkdir apps && cd apps
Create a file named routes.js
```


#### In the 'apps' folder, create a folder named models

```
mkdir models && cd models
Create a file named book.js
```

![Creating Folders](screenshots/creatingfolders.png) 

### Access the Routes with AngularJS  

#### Navigate back to the **Books** directory:  

```
cd ../..
```

#### Create a public folder and move into it:

```
mkdir public && cd public
```

### Inside the public folder, create two files:

script.js → Contains the AngularJS controller configuration.

index.html → Contains the Book Management frontend (form + table).

Go back up to the Books directory and start the server:

```
cd ..
node server.js
```
![Connect MongoDB](screenshots/connectmongodb.png)  

Test locally:

```
curl -s http://localhost:3300
```

Open TCP port 3300 in your AWS EC2 Security Group to allow browser access.

![Edit Inbound Rules](screenshots/editinboundrules.png)  

Access your Book Register application in a browser using:

Public IP:
```
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```

Public DNS:
```
curl -s http://169.254.169.254/latest/meta-data/public-hostname
```
![Book Management Running](screenshots/bookmanagement.png)  








