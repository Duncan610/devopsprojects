# MERN To-Do Application

A simple **To-Do Application** built with the **MERN stack** (MongoDB, Express.js, React.js, Node.js) and deployed on **AWS EC2**.  
This project demonstrates **full-stack web development** combined with **cloud deployment** skills.  

---

## 📌 Features
- Add new tasks to a to-do list
- View all tasks
- Delete tasks
- Simple and responsive UI
- Backend API with MongoDB integration
- Cloud deployment on AWS EC2

---

## ⚙️ Tech Stack
- **MongoDB** – NoSQL document database for storing tasks
- **Express.js** – Backend framework for routing and APIs
- **React.js** – Frontend UI framework
- **Node.js** – JavaScript runtime environment
- **AWS EC2** – Hosting and deployment
- **Nginx** (optional for reverse proxy)
- **GitHub** – Version control

---

## Step 1 – Backend Configuration

In this step, we set up the **backend environment** for our MERN To-Do application on an Ubuntu server.

---

### 🔧 Update and Upgrade Ubuntu
First, update the package lists and upgrade the system:

```
sudo apt update
sudo apt upgrade
```

```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

⚡ Note: The command above installs both Node.js and NPM.

NPM is a package manager for Node (similar to apt for Ubuntu).

It is used to install Node modules, packages, and manage dependency conflicts.

Verify installation:

```
node -v
npm -v
```

📂 Application Code Setup

Create a new project directory:

```
mkdir Todo
ls
```

✅ Use ls -lih to view more detailed information about files and directories.
Run ls --help to explore more useful options.

Navigate into the project directory:

```
cd Todo
```

📄 Initialize Node.js Project

Initialize a new Node.js project using npm init. This creates a package.json file that holds metadata about the project and its dependencies:

```
npm init
```

Press Enter to accept the default values.

When prompted, type yes to confirm and create the file.

Verify that the package.json file was created:

```
ls
```

🚀 Install Express.js

Next, install Express.js, which will be used to build the backend server, and create a routes directory:

```
npm install express
mkdir routes
```

# MERN Web Stack - 103

## Step 1: Install ExpressJS

Express is a framework for Node.js that simplifies backend development. It provides tools to define routes, handle requests, and manage responses.

### Install Express

```
npm install express
```

Create the index.js file
```
touch index.js
ls
```

The ls command should confirm that index.js was created.

Install dotenv

dotenv is used to manage environment variables.

```
npm install dotenv
```

Edit the index.js file

```
nano index.js
```

Paste the following code inside:

```
const express = require('express');
require('dotenv').config();

const app = express();
const port = process.env.PORT || 5000;

app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.use((req, res, next) => {
  res.send('Welcome to Express');
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

Save the file in nano:

Press CTRL + O → Enter (to write/save)

Press CTRL + X (to exit)

Step 2: Start the Server

In the same directory as index.js, run:

```
node index.js
```

If successful, you should see:

Server running on port 5000


Step 3: Allow Port 5000 in Security Group

Update your EC2 Security Group to allow inbound traffic:

```
Type: Custom TCP Rule

Port Range: 5000

Source: 0.0.0.0/0 (Anywhere)

Step 4: Test in Browser

Open your browser and go to:

http://<PublicIP-or-PublicDNS>:5000
```

You should see:

Welcome to Express


To retrieve your server’s details:

Public IP:

```
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```

Public DNS:

```
curl -s http://169.254.169.254/latest/meta-data/public-hostname
```

Step 5: Create Routes

Our To-Do application must handle three actions:

Create a new task (POST)

Display all tasks (GET)

Delete a task (DELETE)

We’ll define these routes next.

Create routes directory

```
mkdir routes
cd routes
```

Create api.js

```
touch api.js
```

```
nano api.js
```

Paste the following starter code:

```
const express = require('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {
  // Get all todos
});

router.post('/todos', (req, res, next) => {
  // Create a new todo
});

router.delete('/todos/:id', (req, res, next) => {
  // Delete a todo by ID
});

module.exports = router;
```

Save and exit with CTRL+O, Enter, CTRL+X.

# MERN Web Stack - 104

## Step 1: Install and Configure Mongoose

Since our application will use **MongoDB** (a NoSQL database), we need to define models and schemas.  
- A **Schema** is a blueprint of how the database will be structured.  
- A **Model** uses this schema to interact with the database.  

We will use **Mongoose**, a Node.js package that simplifies working with MongoDB.

### Install Mongoose
From the root of your `Todo` project:

```
npm install mongoose
```

Step 2: Create the Models Directory

```
mkdir models
cd models
touch todo.js
```

(Alternatively, you can run this in one line:)

```
mkdir models && cd models && touch todo.js
```

Step 3: Define the Todo Model

Open the todo.js file:

```
nano todo.js
```

Paste the following code:

```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

// Create schema for todo
const TodoSchema = new Schema({
  action: {
    type: String,
    required: [true, 'The todo text field is required']
  }
});

// Create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```

Save and exit with:

CTRL + O → Enter (save)

CTRL + X (exit)

Step 4: Update Routes to Use the Model

Navigate back to the routes directory:

```
cd ../routes
```

Open the api.js file:

```
nano api.js
```

Replace its contents with:

```
const express = require('express');
const router = express.Router();
const Todo = require('../models/todo');

// Get all todos
router.get('/todos', (req, res, next) => {
  // Return only the id and action field
  Todo.find({}, 'action')
    .then(data => res.json(data))
    .catch(next);
});

// Create a new todo
router.post('/todos', (req, res, next) => {
  if (req.body.action) {
    Todo.create(req.body)
      .then(data => res.json(data))
      .catch(next);
  } else {
    res.json({ error: "The input field is empty" });
  }
});

// Delete a todo by ID
router.delete('/todos/:id', (req, res, next) => {
  Todo.findOneAndDelete({ "_id": req.params.id })
    .then(data => res.json(data))
    .catch(next);
});

module.exports = router;
```

Save and exit with CTRL+O, Enter, CTRL+X.

# MERN Web Stack - 105

## Step 1: Set Up MongoDB with mLab (MongoDB Atlas)
We need a database to store our data. For this, we will use **MongoDB Atlas** (previously mLab), which provides MongoDB Database-as-a-Service (DBaaS).

1. Go to https://www.mongodb.com/atlas and **sign up** for a free account.  
2. During setup:
   - Select **AWS** as the cloud provider.
   - Choose a region close to you.  
3. Complete the **Get Started Checklist**:
   - Allow access to your MongoDB database from **anywhere (0.0.0.0/0)**.  
     *(⚠️ Not secure for production, but fine for testing)*  
   - Change temporary whitelist access from **6 Hours → 1 Week**.  
4. Create a **Database** and a **Collection** inside your cluster.  

---

## Step 2: Create the `.env` File
Inside the root of your `Todo` project, create a `.env` file:  

```
touch .env
nano .env
```

Paste the following line into the file:

```
DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
```

👉 Replace <username>, <password>, <network-address>, and <dbname> with the actual values from your MongoDB Atlas setup.

Save and exit:

CTRL + O → Enter (save), CTRL + X (exit)

Step 3: Update index.js

We now need to update our index.js file so that it uses the environment variables and connects to MongoDB.

Open the file:

```
nano index.js
```

Delete any existing code inside and paste this:

```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();
const port = process.env.PORT || 5000;

// Connect to the database
mongoose.connect(process.env.DB, {
  useNewUrlParser: true,
  useUnifiedTopology: true
})
.then(() => console.log('Database connected successfully'))
.catch(err => console.log(err));

// Override Mongoose's deprecated promise with Node's promise
mongoose.Promise = global.Promise;

// Middleware
app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.use(bodyParser.json());
app.use('/api', routes);

// Error handling middleware
app.use((err, req, res, next) => {
  console.log(err);
  next();
});

// Start server
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

Save and exit (CTRL+O, Enter, CTRL+X).

Step 4: Start the Server

Run the server:

```
node index.js
```

If everything is set up correctly, you should see:

Database connected successfully
Server running on port 5000


✅ At this point:

Your backend is connected to MongoDB Atlas.

You can now test API routes (GET, POST, DELETE) using Postman.


