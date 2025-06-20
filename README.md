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
