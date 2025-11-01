# 🧾 Express + PostgreSQL CRUD REST API

This project is a **RESTful API** built using **Node.js (Express.js)** and **PostgreSQL**.  
It allows you to perform **CRUD (Create, Read, Update, Delete)** operations on records, secured by basic authentication and documented using **Swagger UI**.

---

## 📋 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Setup Guide](#-setup-guide)
  - [1️⃣ Prerequisites](#1️⃣-prerequisites)
  - [2️⃣ Clone the Repository](#2️⃣-clone-the-repository)
  - [3️⃣ Install Dependencies](#3️⃣-install-dependencies)
  - [4️⃣ Configure Database](#4️⃣-configure-database)
  - [5️⃣ Create Database Tables](#5️⃣-create-database-tables)
  - [6️⃣ Start the Server](#6️⃣-start-the-server)
- [API Endpoints](#-api-endpoints)
- [Swagger Documentation](#-swagger-documentation)
- [Testing with Postman](#-testing-with-postman)
- [Troubleshooting](#-troubleshooting)
- [Future Enhancements](#-future-enhancements)
- [License](#-license)

---

## 🚀 Features

✅ Built using **Express.js**  
✅ Integrated with **PostgreSQL** via `pg`  
✅ CRUD operations on “records” table  
✅ **Authentication middleware** for API access  
✅ **Swagger UI** documentation  
✅ Proper **error handling** and **clean structure**  
✅ Ready to deploy to Render / Heroku

---

## 💻 Tech Stack

| Component | Technology |
|------------|-------------|
| Backend | Node.js + Express |
| Database | PostgreSQL |
| Documentation | Swagger UI (OpenAPI 3.0) |
| Auth | Basic Token Authentication |
| Language | JavaScript (ES6) |
| API Testing | Postman or Swagger |

---

## 📂 Project Structure

│
├── bin/
│ └── www # Server start file
│
├── routes/
│ └── records.js # CRUD routes
│ └── orders.js # CRUD routes
│
├── middleware/
│ └── auth.js # Authentication middleware
│
├── dbconfig.js # PostgreSQL connection
├── app.js # Express app initialization
├── package.json # Dependencies
├── README.md # Documentation
├── swagger.js # Swagger setup
└── swagger.yaml # Swagger Documentation


---


---

## ⚙️ Setup Guide

### 1️⃣ Prerequisites

Ensure you have the following installed:

- [Node.js](https://nodejs.org/en/) (v16+)
- [npm](https://www.npmjs.com/)
- [PostgreSQL](https://www.postgresql.org/) (v13+)

---

### 2️⃣ Clone the Repository

```bash
git clone https://github.com/yourusername/express-crud-api.git
cd express-crud-api


3️⃣ Install Dependencies
npm install


4️⃣ Configure Database

Create a file named dbconfig.js:

// dbconfig.js
const { Pool } = require('pg');

const pool = new Pool({
  user: 'postgres',        // your DB username
  host: 'localhost',
  database: 'recordsdb',   // your DB name
  password: 'yourpassword',
  port: 5432,
});

module.exports = pool;


5️⃣ Create Database Tables

Run the following SQL commands in PostgreSQL:

CREATE DATABASE recordsdb;

\c recordsdb;

CREATE TABLE records (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  status VARCHAR(50) NOT NULL,
  created_at timestamp without time zone
);

Part 2: Database Design and Querying

This part demonstrates the Order Management System (OMS) database design, data population, and essential SQL queries for analytics.
 Database Schema Overview
The OMS database includes the following entities:


Customers → Stores customer details such as name, email, phone, and address.


Products → Contains product catalog with price and stock information.


Orders → Tracks customer orders and order status.


Order_Products → Defines the many-to-many relationship between Orders and Products.


Payments → Records payment information for each order.


Relationships:


One customer can have multiple orders.


One order can include multiple products.


Each order has one or more payments.


 Schema Creation Script
All database tables and relationships are defined in order_management.sql.
It includes DROP, CREATE, and INSERT statements to set up and populate the database.
Main Tables:
TableDescriptioncustomersCustomer informationproductsProduct catalogordersOrder records linked to customersorder_productsMany-to-many link between orders and productspaymentsPayment records for each order
🧠 Sample Data
The schema includes:


5 Customers


10 Orders


10 Products


Associated payments and order-product mappings


 Key SQL Queries
 Top 3 customers with the highest number of orders
SELECT c.name, COUNT(o.order_id) AS total_orders
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.name
ORDER BY total_orders DESC
LIMIT 3;

Finds the most active customers based on total order count.


Retrieve orders placed in the last 30 days
SELECT *
FROM orders
WHERE order_date >= NOW() - INTERVAL '30 days';


⏰ Useful for showing recent orders and activity trends.


3️⃣ Calculate total revenue for each product
SELECT 
    p.name AS product_name,
    SUM(op.quantity * p.price) AS total_revenue
FROM products p
JOIN order_products op ON p.product_id = op.product_id
JOIN orders o ON op.order_id = o.order_id
GROUP BY p.name
ORDER BY total_revenue DESC;


 Helps analyze product performance and sales contribution.


 Design Choices Explained
EntityReason for DesignCustomersBase entity for user info and relationshipsProductsIndependent table to maintain reusable catalogOrdersConnects customers to purchased itemsOrder_ProductsEnables many-to-many linkage between orders & productsPaymentsAllows tracking multiple payment methods per order




Start the Server
npm start


Visit the server at:

https://operation-management-system.onrender.com


🌐 API Endpoints
Method	Endpoint	Description	Example Usage
POST	/records/addRecord	Add new record	/records/addRecord?name=John&status=active
POST	/records/getRecords	Get all or filter by status	/records/getRecords?status=active
PUT	/records/updateRecord	Update record	/records/updateRecord?id=1&name=Jane&status=inactive
DELETE	/records/deleteRecord	Delete record by ID	/records/deleteRecord?id=1

POST	/orders/addCustomer	Add a new customer	/orders/addCustomer
GET	/orders/getCustomers	Retrieve all customers	/orders/getCustomers
POST	/orders/addProduct	Add a new product	/orders/addProduct
GET	/orders/getProducts	Retrieve all products	/orders/getProducts
POST	/orders/addOrder	Create a new order	/orders/addOrder
GET	/orders/getOrders	Retrieve all orders	/orders/getOrders
GET	/orders/getRecentOrders	Get all orders from last 30 days	/orders/getRecentOrders
GET	/orders/topCustomers	Get top 3 customers by total orders	/orders/topCustomers
GET	/orders/productRevenue	Calculate total revenue for each product	/orders/productRevenue
POST	/orders/addPayment	Record a payment for an order	/orders/addPayment
GET	/orders/getPayments	Retrieve all payments	/orders/getPayments


