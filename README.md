SmartStock: Inventory Management System – Capstone Project Proposal

Project Title: Smart Stock: Advanced Inventory Management System
Problem Statement
Managing inventory efficiently is crucial for businesses, but many small and medium-sized enterprises (SMEs) still rely on manual tracking or outdated systems, leading to stock discrepancies, lost sales, and operational inefficiencies.

Why is this problem significant?
•	Manual inventory tracking is error-prone and time-consuming.
•	Poor stock visibility can lead to overstocking or stockouts.
•	Lack of real-time data makes decision-making difficult.
•	Existing solutions may be too complex or expensive for small businesses.

This project aims to provide a user-friendly, cost-effective inventory management system that helps businesses track stock levels, manage suppliers, and generate reports—all in one place.

________________________________________
Overview of the Application’s Functionality
Smart Stock: Is a full-stack web application that allows businesses to:
•	Track inventory in real-time with stock level alerts.
•	Manage suppliers and purchase orders.
•	Generate reports (sales trends, stock valuation, etc.).
•	Control user access with role-based permissions (Admin, Manager, Staff).
•	Scan barcodes for quick product lookup.

The system will have:
•	A React.js frontend with a clean, responsive UI.
•	A Node.js + Express backend with a MongoDB database.
•	JWT authentication for secure login.
•	RESTful APIs for data exchange.

________________________________________
Technology Stack
Category	Technologies
Frontend	React.js, TypeScript, Material-UI, React Query, Formik + Yup
Backend	Node.js, Express.js, MongoDB (Mongoose), JWT
DevOps	Git, GitHub, Vercel (Frontend Hosting), Render (Backend Hosting)
Testing	Jest (Unit Testing), Postman (API Testing)
Additional Tools	Barcode.js (for barcode generation), Chart.js (for analytics)
________________________________________
Features to be Implemented
Core Features (MVP - Minimum Viable Product)
Product Management – Add, edit, delete, and categorize products.
Inventory Tracking – Real-time stock updates with low-stock alerts.
Supplier Management – Track supplier details and purchase orders.
User Authentication – Login, logout, and role-based access (Admin, Staff).
Basic Dashboard – Summary of stock levels and recent activities.

Additional Features (Future Enhancements)
🔹 Barcode/QR Scanning – Scan products using a webcam or mobile.
🔹 Advanced Reporting – Export data to Excel/PDF, sales analytics.
🔹 Multi-Warehouse Support – Track inventory across multiple locations.
🔹 Mobile App Integration – Sync with a React Native app.
________________________________________

User Stories
1.	As an Admin, I want to add new products to the inventory so that I can keep track of available stock.
2.	As a Store Manager, I want to receive low-stock alerts so that I can reorder products before they run out.
3.	As a Staff Member, I want to update stock levels after sales so that inventory records stay accurate.
4.	As a Supplier, I want to view purchase orders so that I know what to deliver.
5.	As a Business Owner, I want to generate sales reports so that I can analyze business performance.

________________________________________
Instructor Feedback & Approval
•	Submitted for review on [05/22/2025].
•	Final approval received on [05/22/2025].
________________________________________





SmartStock: Design Architecture & API Contract
High-Level Architecture
System Architecture Diagram
┌───────────────────────────────────────────────────────────────────────┐
│                           SmartStock System                                                                          │
├─────────────────┐       ├─────────────────┐       ├─────────────────┐
│   Frontend      │       │   API Gateway   │       │   External      │
│ (React SPA)     │◄─────►│ (Express.js)    │◄─────►│   Services     │
└─────────────────┘       └─────────────────┘       └─────────────────┘
                                      ▲
                                      │
                      ┌───────────────┴───────────────┐
                      │                               │
            ┌─────────────────┐             ┌─────────────────┐
            │   Microservices │             │   Database      │
            │   - Auth       │             │   (MongoDB)     │
            │   - Inventory  │             │                 │
            │   - Reporting  │             │                 │
            └─────────────────┘             └─────────────────┘

Component Breakdown
•	Frontend: React SPA with Material-UI
•	API Gateway: Express.js router
•	Microservices:
o	Auth Service (JWT)
o	Inventory Service
o	Reporting Service
•	Database: MongoDB Atlas
•	External Integrations:
o	Barcode Scanner API
o	Email/SMS Alert Service
 
 ![image](https://github.com/user-attachments/assets/05733507-8946-4bc6-9ff8-440f94bad284)

erDiagram
    USER ||--o{ PRODUCT : creates
    USER ||--o{ SUPPLIER : manages
    PRODUCT }|--|| SUPPLIER : supplied_by
    PRODUCT ||--o{ STOCK_MOVEMENT : has

    PRODUCT {
        string _id PK
        string name
        string sku
        string category
        number currentStock
        number lowStockThreshold
        date expiryDate
        objectId supplierId FK
    }

    SUPPLIER {
        string _id PK
        string name
        string contactEmail
        string phone
    }

    STOCK_MOVEMENT {
        string _id PK
        objectId productId FK
        number quantity
        string movementType
        date timestamp
    }

    USER {
        string _id PK
        string email
        string passwordHash
        string role
    }



Collections Description
Collection	Fields	Relationships
products	_id, name, sku, category, currentStock, expiryDate, supplierId	Belongs to supplier
suppliers	_id, name, contactEmail, phone	Has many products
stock_movements	_id, productId, quantity, movementType, timestamp	References product
users	_id, email, passwordHash, role (admin/manager/staff)	-


API Contract
Base URL: https://api.smartstock.in/v1

Authentication:
POST /auth/login
# Request
{
  "email": "admin@example.com",
  "password": "securepassword"
}

# Response
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "user123",
    "email": "admin@example.com",
    "role": "admin"
  }
}

Products:
GET /products
# Response
{
  "data": [
    {
      "_id": "prod123",
      "name": "Organic Milk",
      "sku": "MILK001",
      "category": "dairy",
      "currentStock": 42,
      "lowStockThreshold": 20,
      "expiryDate": "2023-12-31",
      "supplier": {
        "_id": "supp456",
        "name": "Dairy Farms Inc."
      }
    }
  ],
  "pagination": {
    "total": 1,
    "page": 1,
    "pages": 1
  }
}

POST /products
# Request
{
  "name": "New Product",
  "sku": "SKU123",
  "category": "dry goods",
  "initialStock": 100,
  "lowStockThreshold": 20,
  "supplierId": "supp456"
}

Inventory:
PATCH /inventory/{productId}/adjust
# Request
{
  "adjustment": -5,
  "reason": "sale"
}

# Response
{
  "_id": "prod123",
  "name": "Organic Milk",
  "currentStock": 37,
  "updatedAt": "2023-05-22T12:34:56.789Z"
}

Reporting:
GET /reports/expiry-alerts
# Response
{
  "critical": [
    { 
      "productId": "prod789",
      "product": "Yogurt", 
      "daysUntilExpiry": 2,
      "currentStock": 15
    }
  ],
  "warning": [
    {
      "productId": "prod456",
      "product": "Cheese",
      "daysUntilExpiry": 14,
      "currentStock": 8
    }
  ]
}

GET /reports/stock-levels
# Response
{
  "lowStock": [
    {
      "productId": "prod456",
      "name": "Cheese",
      "currentStock": 8,
      "threshold": 10
    }
  ],
  "outOfStock": []
}

# FSWD-Project
