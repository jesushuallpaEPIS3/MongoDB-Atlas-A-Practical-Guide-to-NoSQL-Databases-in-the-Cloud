# Building an Application with MongoDB Atlas: A Practical Guide to NoSQL Databases in the Cloud

**JESUS ANTONIO HUALLPA MARON**  
*Dec 12, 2024*  
*3 min read*

## Introduction
In an increasingly digital world, NoSQL databases have gained popularity for their ability to handle large volumes of data and scale horizontally in modern applications. One of the most versatile and accessible solutions is MongoDB Atlas, a fully managed NoSQL database platform in the cloud. In this article, we will explore how to build an application using MongoDB Atlas as a backend for CRUD operations.

## What is MongoDB Atlas?

MongoDB Atlas is a cloud database service that offers the ease of configuring, scaling, and managing MongoDB databases on various cloud platforms such as AWS, Azure, and Google Cloud Platform. Atlas allows you to handle data in JSON-like format, which makes it easy to integrate with modern applications.

## Step-by-Step: Building the Application

### Setting Up MongoDB Atlas

1. **Create an Account**: Go to MongoDB Atlas and sign up.

2. **Configure a Cluster**:
   - Select a cloud provider (AWS, GCP, or Azure).
   - Configure basic cluster options (region, virtual machine, etc.).
   - Click "Create Cluster."

3. **Set Up Access**:
   - Create a database user with a username and password.
   - Allow access from your IP by configuring the firewall.

4. **Create a Database**:
   - Use the "Collections" option to add a database and an initial collection.

### Setting Up the Local Environment

1. **Install Dependencies**:
   - Ensure you have Node.js and npm installed.
   - Install the MongoDB client: `npm install mongoose`.

2. **Connect to MongoDB Atlas**:
   - Get the connection URI from Atlas.
   - Configure a `.env` file to store the URI securely.

3. **Create the Connection File**:

```javascript
const mongoose = require('mongoose'); 
require('dotenv').config();

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    });
    console.log('MongoDB Connected...');
  } catch (err) {
    console.error(err.message);
    process.exit(1);
  }
};

module.exports = connectDB;
```
### 3. Create a Backend with Express

1. **Initialize a Project**:
   - Run: `npm init -y`.

2. **Install Express**:
   - Run: `npm install express`.

3. **Set Up the Server**:

```javascript
const express = require('express');
const connectDB = require('./config/db');

const app = express();
connectDB();

app.use(express.json());

const PORT = process.env.PORT || 5000;

app.listen(PORT, () => console.log(`Server running on port ${PORT}`));

```
4. Create CRUD Operations

```javascript
const ItemSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
  },
  description: {
    type: String,
  },
  date: {
    type: Date,
    default: Date.now,
  },
});

module.exports = mongoose.model('Item', ItemSchema);
```
CRUD Routes:

```javascript
const express = require('express');
const router = express.Router();
const Item = require('./models/Item');

// Create an item
router.post('/', async (req, res) => {
  try {
    const newItem = new Item(req.body);
    const item = await newItem.save();
    res.json(item);
  } catch (err) {
    res.status(500).send('Server Error');
  }
});

// Read all items
router.get('/', async (req, res) => {
  try {
    const items = await Item.find();
    res.json(items);
  } catch (err) {
    res.status(500).send('Server Error');
  }
});

// Update an item
router.put('/:id', async (req, res) => {
  try {
    const item = await Item.findByIdAndUpdate(req.params.id, req.body, {
      new: true,
    });
    res.json(item);
  } catch (err) {
    res.status(500).send('Server Error');
  }
});

// Delete an item
router.delete('/:id', async (req, res) => {
  try {
    await Item.findByIdAndDelete(req.params.id);
    res.json({ msg: 'Item deleted' });
  } catch (err) {
    res.status(500).send('Server Error');
  }
});

module.exports = router;
```
Integrate the Frontend (Optional)
You can use React, Angular, or any other framework to create an interface that consumes these routes.

Conclusion
MongoDB Atlas significantly simplifies the management of cloud NoSQL databases, allowing developers to focus on application logic. By following this guide, you will have created a functional application with full CRUD support.
