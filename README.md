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
