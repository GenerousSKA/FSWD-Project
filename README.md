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





Milestone Two Assignment: Design & API Contract Development

Objective: Design the application’s architecture, database schema, and develop API contracts.

High-Level Architecture
Components Breakdown
Frontend: React SPA with Material-UI
API Gateway: Express.js router
Microservices:
o	Auth Service (JWT)
o	Inventory Service
o	Reporting Service
Database: MongoDB Atlas
External Integrations:
o	Barcode Scanner API
o	Email/SMS Alert Service



![image](https://github.com/user-attachments/assets/7d7624f0-104a-4c5e-9548-01a6e2b24c1c)


 
Database Schema Design
o	Define the necessary tables, relationships, and entities for the app.
o	Create ER diagrams (entity-relationship) and discuss CRUD operations.


![image](https://github.com/user-attachments/assets/43d3ea63-6db1-41f1-8040-b1b619b0188f)



 
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
users	_id, email, passwordHash, role (admin/manager/staff)	



API Contract
o	Define the API endpoints, request/response format, and authorization needs.

Base URL: https://api.smartstock.in/v1
Products

GET /products
# Response
{	
  "data": [
    {
      "_id": "prod123",
      "name": "Organic Milk",
      "currentStock": 42,
      "lowStockThreshold": 20,
      "expiryDate": "2023-12-31"
    }
  ]
}


POST /products
# Request
{
  "name": "New Product",
  "sku": "SKU123",
  "initialStock": 100
}

Inventory

PATCH /inventory/{productId}/adjust
# Request
{
  "adjustment": -5,
  "reason": "sale"
}

Reporting

GET /reports/expiry-alerts
# Response
{
  "critical": [
    { "product": "Yogurt", "daysUntilExpiry": 2 }
  ],
  "warning": []
}

