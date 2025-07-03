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






Milestone Three Project: Development – Backend Implementation

Objective:
Start implementing the core functionalities of the application, focusing on backend development first.

Node.js installed (version 18 or higher)
MongoDB Atlas account (free tier is sufficient)
Postman installed for API testing

Set Up the Backend Server
o	mkdir smartstock-backend
o	cd smartstock-backend
o	npm init -y
o	npm install express mongoose jsonwebtoken bcryptjs cors dotenv
o	npm install --save-dev nodemon

Create basic file structure
/smartstock-backend
  ├── /src
  │   ├── /config
  │   ├── /controllers
  │   ├── /models
  │   ├── /routes
  │   ├── /middleware
  │   ├── app.js
  │   └── server.js
  ├── .env
  └── package.json

Set up the basic Express server
const app = require('./app');
const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});

const express = require('express');
const cors = require('cors');
const app = express();

app.use(cors());
app.use(express.json());

app.get('/', (req, res) => {
  res.send('SmartStock Backend is running!');
});
module.exports = app;

Integrate MongoDB Database
Set up MongoDB Atlas
o	Go to MongoDB Atlas
o	Create a free account if you don't have one
o	Create a new cluster
o	Create a database user with read/write permissions
o	Whitelist your IP address (or use 0.0.0.0/0 for development)
o	Get your connection string

Configure database connection
o	MONGODB_URI=your_mongodb_connection_string
o	JWT_SECRET=your_random_secret_key

const mongoose = require('mongoose');
require('dotenv').config();
const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGODB_URI);
    console.log('MongoDB connected successfully');
  } catch (error) {
    console.error('MongoDB connection error:', error);
    process.exit(1);
  }
};
module.exports = connectDB;
Update server.js to connect to MongoDB:

const connectDB = require('./config/db');
const app = require('./app');
const PORT = process.env.PORT || 3000;

connectDB();

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});

Create Database Models
const mongoose = require('mongoose');
const bcrypt = require('bcryptjs');

const userSchema = new mongoose.Schema({
  email: { type: String, required: true, unique: true },
  passwordHash: { type: String, required: true },
  role: { type: String, enum: ['admin', 'manager', 'staff'], required: true }
});

userSchema.pre('save', async function(next) {
  if (this.isModified('passwordHash')) {
    this.passwordHash = await bcrypt.hash(this.passwordHash, 10);
  }
  next();
});

module.exports = mongoose.model('User', userSchema);

const mongoose = require('mongoose');
const supplierSchema = new mongoose.Schema({
  name: { type: String, required: true },
  contactEmail: { type: String, required: true },
  phone: { type: String, required: true }
});
module.exports = mongoose.model('Supplier', supplierSchema);
const mongoose = require('mongoose');

const productSchema = new mongoose.Schema({
  name: { type: String, required: true },
  sku: { type: String, required: true, unique: true },
  category: { type: String, required: true },
  currentStock: { type: Number, required: true, min: 0 },
  lowStockThreshold: { type: Number, required: true, min: 0 },
  expiryDate: { type: Date },
  supplierId: { 
    type: mongoose.Schema.Types.ObjectId, 
    ref: 'Supplier', 
    required: true 
  }
});

module.exports = mongoose.model('Product', productSchema);

const mongoose = require('mongoose');
const stockMovementSchema = new mongoose.Schema({
  productId: { 
    type: mongoose.Schema.Types.ObjectId, 
    ref: 'Product', 
    required: true 
  },
  quantity: { type: Number, required: true },
  movementType: { 
    type: String, 
    enum: ['purchase', 'sale', 'adjustment', 'return'], 
    required: true 
  },
  timestamp: { type: Date, default: Date.now }
});
module.exports = mongoose.model('StockMovement', stockMovementSchema);

Implement API Routes
Set up authentication middleware
const jwt = require('jsonwebtoken');
const User = require('../models/User');

const auth = async (req, res, next) => {
  try {
    const token = req.header('Authorization').replace('Bearer ', '');
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    const user = await User.findById(decoded.userId);

    if (!user) {
      throw new Error();
    }

    req.user = user;
    next();
  } catch (error) {
    res.status(401).send({ error: 'Please authenticate.' });
  }
};
module.exports = auth;

Create authentication routes
const express = require('express');
const router = express.Router();
const jwt = require('jsonwebtoken');
const User = require('../models/User');

router.post('/login', async (req, res) => {
  try {
    const { email, password } = req.body;
    const user = await User.findOne({ email });

    if (!user || !(await user.comparePassword(password))) {
      return res.status(401).json({ error: 'Invalid credentials' });
    }
const token = jwt.sign(
      { userId: user._id, role: user.role },
      process.env.JWT_SECRET,
      { expiresIn: '24h' }
    );

    res.json({ token });
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});
module.exports = router;

Create product routes
const express = require('express');
const router = express.Router();
const Product = require('../models/Product');
const auth = require('../middleware/auth');
router.get('/', auth, async (req, res) => {
  try {
    const products = await Product.find().populate('supplierId', 'name contactEmail');
    res.json({ data: products });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
router.post('/', auth, async (req, res) => {
  try {
    const { name, sku, initialStock, ...rest } = req.body;
    const product = new Product({
      name,
      sku,
      currentStock: initialStock,
      ...rest
    });
    
    await product.save();
    res.status(201).json(product);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});
module.exports = router;

Create inventory routes

const express = require('express');
const router = express.Router();
const Product = require('../models/Product');
const StockMovement = require('../models/StockMovement');
const auth = require('../middleware/auth');

router.patch('/:productId/adjust', auth, async (req, res) => {
  try {
    const { adjustment, reason } = req.body;
    const product = await Product.findById(req.params.productId);

    if (!product) {
      return res.status(404).json({ error: 'Product not found' });
    }

    product.currentStock += adjustment;
    if (product.currentStock < 0) {
      return res.status(400).json({ error: 'Insufficient stock' });
    }

    const movement = new StockMovement({
      productId: product._id,
      quantity: Math.abs(adjustment),
      movementType: reason === 'sale' ? 'sale' : 'adjustment'
    });

    await Promise.all([product.save(), movement.save()]);
    res.json({ 
      message: 'Inventory adjusted successfully',
      currentStock: product.currentStock
    });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
module.exports = router;

Create reporting routes
const express = require('express');
const router = express.Router();
const Product = require('../models/Product');
const auth = require('../middleware/auth');

router.get('/expiry-alerts', auth, async (req, res) => {
  try {
    const products = await Product.find({ expiryDate: { $exists: true } });
    const today = new Date();
    const critical = [];
    const warning = [];
     products.forEach(product => {
      const expiryDate = new Date(product.expiryDate);
      const daysUntilExpiry = Math.ceil((expiryDate - today) / (1000 * 60 * 60 * 24));
      
      if (daysUntilExpiry <= 7) {
        const entry = {
          product: product.name,
          daysUntilExpiry,
          currentStock: product.currentStock
        };
        
        if (daysUntilExpiry <= 3) {
          critical.push(entry);
        } else {
          warning.push(entry);
        }
      }
    });
    
    res.json({ critical, warning });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
module.exports = router;

Register all routes in app.js
const express = require('express');
const cors = require('cors');
const authRoutes = require('./routes/auth');
const productRoutes = require('./routes/products');
const inventoryRoutes = require('./routes/inventory');
const reportRoutes = require('./routes/reports');
const app = express();

app.use(cors());
app.use(express.json());

app.use('/auth', authRoutes);
app.use('/products', productRoutes);
app.use('/inventory', inventoryRoutes);
app.use('/reports', reportRoutes);
app.get('/', (req, res) => {
  res.send('SmartStock Backend is running!');
});
module.exports = app;


Testing API with Postman

Creating a test user
o	create a user in MongoDB:

{
  "email": "admin@smartstock.com",
  "passwordHash": "plaintextpassword", // This will be hashed automatically
  "role": "admin"
}

Test the authentication flow
Login Request
Method: POST
URL: http://localhost:3000/auth/login
Body (raw JSON):

json
{
  "email": "admin@smartstock.com",
  "password": "plaintextpassword"
}

Save the token from the response

Create Product Request
Method: POST
URL: http://localhost:3000/products
Headers:
Authorization: Bearer <your_token>
Content-Type: application/json
Body (raw JSON):

json
{
  "name": "Organic Milk",
  "sku": "MILK001",
  "category": "Dairy",
  "initialStock": 50,
  "lowStockThreshold": 10,
  "expiryDate": "2023-12-31",
  "supplierId": "507f1f77bcf86cd799439011"
}

Get Products Request
Method: GET
URL: http://localhost:3000/products
Headers:
Authorization: Bearer <your_token>

Adjust Inventory Request
Method: PATCH
URL: http://localhost:3000/inventory/<product_id>/adjust
Headers:
Authorization: Bearer <your_token>
Content-Type: application/json

Body (raw JSON):

json
{
  "adjustment": -5,
  "reason": "sale"
}

Get Expiry Alerts Request
Method: GET
URL: http://localhost:3000/reports/expiry-alerts
Headers:
Authorization: Bearer <your_token>













Milestone Four Project: Development – Frontend Implementation
Objective:
Start implementing the core functionalities of the application, focusing on the frontend.
Frontend Development
Create React App
o	npx create-react-app smartstock-frontend
o	cd smartstock-frontend

Install Dependencies
o	npm install @mui/material @emotion/react @emotion/styled @mui/icons-material axios react-router-dom formik yup

File Structure
smartstock-frontend/
├── public/
├── src/
│   ├── components/
│   │   ├── Layout/
│   │   │   ├── Navbar.jsx
│   │   │   └── Sidebar.jsx
│   │   ├── Products/
│   │   │   ├── ProductList.jsx
│   │   │   ├── ProductForm.jsx
│   │   │   └── ProductCard.jsx
│   │   ├── Inventory/
│   │   │   └── AdjustInventory.jsx
│   │   └── common/
│   │       ├── Alert.jsx
│   │       └── Loading.jsx
│   ├── pages/
│   │   ├── Login.jsx
│   │   ├── Dashboard.jsx
│   │   ├── Products.jsx
│   │   ├── Inventory.jsx
│   │   └── Reports.jsx
│   ├── services/
│   │   ├── api.js
│   │   └── auth.js
│   ├── App.js
│   ├── index.js
│   └── theme.js
├── .env
└── package.json

Configure API Service
Create API service
import axios from 'axios';
const api = axios.create({
  baseURL: process.env.REACT_APP_API_URL || 'http://localhost:3000',
});

api.interceptors.request.use((config) => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

export default api;

Create auth service
import api from './api';
export const login = async (email, password) => {
  try {
    const response = await api.post('/auth/login', { email, password });
    localStorage.setItem('token', response.data.token);
    return response.data.user;
  } catch (error) {
    throw error.response?.data?.error || 'Login failed';
  }
};
export const logout = () => {
  localStorage.removeItem('token');
};

Implement Authentication
Create Login Page
import { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import { Button, TextField, Container, Typography, Box } from '@mui/material';
import { login } from '../services/auth';
const Login = () => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');
  const navigate = useNavigate();

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      await login(email, password);
      navigate('/dashboard');
    } catch (err) {
      setError(err);
    }
  };

  return (
    <Container maxWidth="xs">
      <Box sx={{ mt: 8, display: 'flex', flexDirection: 'column', alignItems: 'center' }}>
        <Typography component="h1" variant="h5">
          SmartStock Login
        </Typography>
        {error && (
          <Typography color="error" sx={{ mt: 2 }}>
            {error}
          </Typography>
        )}
        <Box component="form" onSubmit={handleSubmit} sx={{ mt: 1 }}>
          <TextField
            margin="normal"
            required
            fullWidth
            label="Email"
            autoComplete="email"
            autoFocus
            value={email}
            onChange={(e) => setEmail(e.target.value)}
          />
          <TextField
            margin="normal"
            required
            fullWidth
            label="Password"
            type="password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
          />
          <Button
            type="submit"
            fullWidth
            variant="contained"
            sx={{ mt: 3, mb: 2 }}
          >
            Sign In
          </Button>
        </Box>
      </Box>
    </Container>
  );
};

export default Login;

Create Protected Route Component
import { Navigate } from 'react-router-dom';
const ProtectedRoute = ({ children }) => {
  const token = localStorage.getItem('token');
  
  if (!token) {
    return <Navigate to="/login" replace />;
  }

  return children;
};

export default ProtectedRoute;


Build Main Layout
Create Navbar

import { AppBar, Toolbar, Typography, Button } from '@mui/material';
import { logout } from '../../services/auth';
import { useNavigate } from 'react-router-dom';
const Navbar = () => {
  const navigate = useNavigate();

  const handleLogout = () => {
    logout();
    navigate('/login');
  };

  return (
    <AppBar position="static">
      <Toolbar>
        <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
          SmartStock
        </Typography>
        <Button color="inherit" onClick={handleLogout}>
          Logout
        </Button>
      </Toolbar>
    </AppBar>
  );
};

export default Navbar;

Create Sidebar
import { Drawer, List, ListItem, ListItemIcon, ListItemText } from '@mui/material';
import { Inventory, ListAlt, Report, Dashboard } from '@mui/icons-material';
import { Link } from 'react-router-dom';

const Sidebar = () => {
  const menuItems = [
    { text: 'Dashboard', icon: <Dashboard />, path: '/dashboard' },
    { text: 'Products', icon: <ListAlt />, path: '/products' },
    { text: 'Inventory', icon: <Inventory />, path: '/inventory' },
    { text: 'Reports', icon: <Report />, path: '/reports' },
  ];

  return (
    <Drawer variant="permanent" anchor="left">
      <List>
        {menuItems.map((item) => (
          <ListItem button key={item.text} component={Link} to={item.path}>
            <ListItemIcon>{item.icon}</ListItemIcon>
            <ListItemText primary={item.text} />
          </ListItem>
        ))}
      </List>
    </Drawer>
  );
};

export default Sidebar;

Implement Product Management
Create Product List Page

import { useEffect, useState } from 'react';
import { Box, Button, Container, Typography } from '@mui/material';
import { Link } from 'react-router-dom';
import api from '../services/api';
import ProductList from '../components/Products/ProductList';
const Products = () => {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState('');

  useEffect(() => {
    const fetchProducts = async () => {
      try {
        const response = await api.get('/products');
        setProducts(response.data.data);
      } catch (err) {
        setError('Failed to fetch products');
      } finally {
        setLoading(false);
      }
    };

    fetchProducts();
  }, []);

  if (loading) return <Typography>Loading...</Typography>;
  if (error) return <Typography color="error">{error}</Typography>;

  return (
    <Container>
      <Box sx={{ display: 'flex', justifyContent: 'space-between', mb: 2 }}>
        <Typography variant="h4">Products</Typography>
        <Button variant="contained" component={Link} to="/products/new">
          Add Product
        </Button>
      </Box>
      <ProductList products={products} />
    </Container>
  );
};

export default Products;

Create Product Form
import { useState, useEffect } from 'react';
import { useNavigate, useParams } from 'react-router-dom';
import { TextField, Button, Box, Typography, MenuItem } from '@mui/material';
import { Formik } from 'formik';
import * as Yup from 'yup';
import api from '../../services/api';

const ProductForm = () => {
  const { id } = useParams();
  const navigate = useNavigate();
  const [suppliers, setSuppliers] = useState([]);
  const [isEdit, setIsEdit] = useState(false);

  useEffect(() => {
    const fetchSuppliers = async () => {
      const response = await api.get('/suppliers');
      setSuppliers(response.data);
    };

    fetchSuppliers();

    if (id) {
      setIsEdit(true);
      // Fetch product data if editing
    }
  }, [id]);

  const validationSchema = Yup.object().shape({
    name: Yup.string().required('Required'),
    sku: Yup.string().required('Required'),
    category: Yup.string().required('Required'),
    initialStock: Yup.number().min(0, 'Must be positive').required('Required'),
    lowStockThreshold: Yup.number().min(0, 'Must be positive').required('Required'),
    supplierId: Yup.string().required('Required'),
  });

  const handleSubmit = async (values) => {
    try {
      if (isEdit) {
        await api.put(`/products/${id}`, values);
      } else {
        await api.post('/products', values);
      }
      navigate('/products');
    } catch (error) {
      console.error('Error saving product:', error);
    }
  };

  return (
    <Box sx={{ p: 3 }}>
      <Typography variant="h5" gutterBottom>
        {isEdit ? 'Edit Product' : 'Add New Product'}
      </Typography>
      <Formik
        initialValues={{
          name: '',
          sku: '',
          category: '',
          initialStock: 0,
          lowStockThreshold: 5,
          expiryDate: '',
          supplierId: '',
        }}
        validationSchema={validationSchema}
        onSubmit={handleSubmit}
      >
        {({ values, errors, touched, handleChange, handleSubmit }) => (
          <form onSubmit={handleSubmit}>
            <TextField
              fullWidth
              margin="normal"
              label="Product Name"
              name="name"
              value={values.name}
              onChange={handleChange}
              error={touched.name && Boolean(errors.name)}
              helperText={touched.name && errors.name}
            />
            {/* Add other form fields similarly */}
            <Button type="submit" variant="contained" sx={{ mt: 2 }}>
              Save
            </Button>
          </form>
        )}
      </Formik>
    </Box>
  );
};

export default ProductForm;

Connect to Backend API
Update App.js

import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { CssBaseline, ThemeProvider } from '@mui/material';
import theme from './theme';
import Navbar from './components/Layout/Navbar';
import Sidebar from './components/Layout/Sidebar';
import Login from './pages/Login';
import Dashboard from './pages/Dashboard';
import Products from './pages/Products';
import Inventory from './pages/Inventory';
import Reports from './pages/Reports';
import ProtectedRoute from './components/common/ProtectedRoute';
const App = () => {
  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      <BrowserRouter>
        <Routes>
          <Route path="/login" element={<Login />} />
          <Route
            path="/*"
            element={
              <ProtectedRoute>
                <Navbar />
                <Box sx={{ display: 'flex' }}>
                  <Sidebar />
                  <Box component="main" sx={{ flexGrow: 1, p: 3 }}>
                    <Routes>
                      <Route path="/dashboard" element={<Dashboard />} />
                      <Route path="/products" element={<Products />} />
                      <Route path="/products/new" element={<ProductForm />} />
                      <Route path="/products/:id" element={<ProductForm />} />
                      <Route path="/inventory" element={<Inventory />} />
                      <Route path="/reports" element={<Reports />} />
                    </Routes>
                  </Box>
                </Box>
              </ProtectedRoute>
            }
          />
        </Routes>
      </BrowserRouter>
    </ThemeProvider>
  );
};

export default App;

Implement Inventory Adjustment
Create Inventory Adjustment Component

import { useState } from 'react';
import { TextField, Button, Box, Typography, MenuItem } from '@mui/material';
import api from '../../services/api';

const AdjustInventory = ({ productId }) => {
  const [adjustment, setAdjustment] = useState(0);
  const [reason, setReason] = useState('adjustment');
  const [message, setMessage] = useState('');
const handleSubmit = async () => {
    try {
      await api.patch(`/inventory/${productId}/adjust`, { adjustment, reason });
      setMessage('Inventory adjusted successfully');
    } catch (error) {
      setMessage('Failed to adjust inventory');
    }
  };

  return (
    <Box sx={{ p: 2 }}>
      <Typography variant="h6">Adjust Inventory</Typography>
      <TextField
        label="Adjustment"
        type="number"
        value={adjustment}
        onChange={(e) => setAdjustment(Number(e.target.value))}
        fullWidth
        margin="normal"
      />
      <TextField
        select
        label="Reason"
        value={reason}
        onChange={(e) => setReason(e.target.value)}
        fullWidth
        margin="normal"
      >
        <MenuItem value="purchase">Purchase</MenuItem>
        <MenuItem value="sale">Sale</MenuItem>
        <MenuItem value="adjustment">Adjustment</MenuItem>
        <MenuItem value="return">Return</MenuItem>
      </TextField>
      <Button variant="contained" onClick={handleSubmit} sx={{ mt: 2 }}>
        Submit
      </Button>
      {message && (
        <Typography sx={{ mt: 2 }} color={message.includes('success') ? 'success' : 'error'}>
          {message}
        </Typography>
      )}
    </Box>
  );
};

export default AdjustInventory;

Display Reports
Create Reports Page

import { useState, useEffect } from 'react';
import { Box, Typography, Paper, Table, TableBody, TableCell, TableContainer, TableHead, TableRow } from '@mui/material';
import api from '../services/api';

const Reports = () => {
  const [expiryAlerts, setExpiryAlerts] = useState({ critical: [], warning: [] });
  const [stockLevels, setStockLevels] = useState({ lowStock: [], outOfStock: [] });

  useEffect(() => {
    const fetchData = async () => {
      try {
        const expiryResponse = await api.get('/reports/expiry-alerts');
        setExpiryAlerts(expiryResponse.data);
        
        const stockResponse = await api.get('/reports/stock-levels');
        setStockLevels(stockResponse.data);
      } catch (error) {
        console.error('Error fetching reports:', error);
      }
    };

    fetchData();
  }, []);

  return (
    <Box sx={{ p: 3 }}>
      <Typography variant="h4" gutterBottom>
        Reports
      </Typography>
      
      <Typography variant="h5" sx={{ mt: 3 }}>Expiry Alerts</Typography>
      <Box sx={{ display: 'flex', gap: 2, mt: 2 }}>
        <Paper sx={{ p: 2, flex: 1 }}>
          <Typography variant="h6" color="error">Critical ({expiryAlerts.critical.length})</Typography>
          <TableContainer>
            <Table>
              <TableHead>
                <TableRow>
                  <TableCell>Product</TableCell>
                  <TableCell>Days Until Expiry</TableCell>
                  <TableCell>Current Stock</TableCell>
                </TableRow>
              </TableHead>
              <TableBody>
                {expiryAlerts.critical.map((item, index) => (
                  <TableRow key={index}>
                    <TableCell>{item.product}</TableCell>
                    <TableCell>{item.daysUntilExpiry}</TableCell>
                    <TableCell>{item.currentStock}</TableCell>
                  </TableRow>
                ))}
              </TableBody>
            </Table>
          </TableContainer>
        </Paper>
        
        <Paper sx={{ p: 2, flex: 1 }}>
          <Typography variant="h6" color="warning.main">Warning ({expiryAlerts.warning.length})</Typography>
          {/* Similar table for warning items */}
        </Paper>
      </Box>

      <Typography variant="h5" sx={{ mt: 4 }}>Stock Levels</Typography>
      {/* Similar tables for low stock and out of stock items */}
    </Box>
  );
};

export default Reports;

Final Steps: Create .env file
o	REACT_APP_API_URL=http://localhost:3000
o	npm start

![SmartStock Login Image](https://github.com/user-attachments/assets/e2cb7dd0-e80d-43c8-ad54-8ad4351a39f3)


![SmartStock Dashboard](https://github.com/user-attachments/assets/b8da301f-08a4-4e07-9fe6-351896afb763)


![SmartStock Inventory](https://github.com/user-attachments/assets/1a497311-b081-43f8-9b8a-b8e17071366f)


![SmartStock Report](https://github.com/user-attachments/assets/ad85ef33-15d0-40fb-a06a-57ee44a0cd16)
