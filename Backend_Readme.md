# License Management System - Backend Documentation

## Table of Contents
1. [Overview](#overview)
2. [System Architecture](#system-architecture)
3. [Database Schema](#database-schema)
4. [API Endpoints](#api-endpoints)
5. [Authentication & Authorization](#authentication--authorization)
6. [Core Features](#core-features)
7. [Setup & Configuration](#setup--configuration)
8. [Error Handling](#error-handling)
9. [Security Considerations](#security-considerations)

## Overview

The License Management System (LMS) Backend is a Node.js/Express.js application that provides APIs for managing software licenses, products, users, and purchases. It serves as the backbone for a system that handles software license distribution, validation, and management.

## System Architecture

The backend follows a standard MVC (Model-View-Controller) architecture with the following components:

- **Models**: Define the data structure and database schema
- **Controllers**: Handle business logic and request processing
- **Routes**: Define API endpoints and map them to controllers
- **Middleware**: Handle cross-cutting concerns like authentication and authorization

### Tech Stack

- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB (with Mongoose ODM)
- **Authentication**: JWT (JSON Web Tokens)
- **File Upload**: Multer
- **Email**: Nodemailer
- **Hashing**: bcrypt

## Database Schema

### 1. User Model
```javascript
{
  email: String,        // Unique email address
  name: String,         // User's full name
  role: String,         // 'admin' or 'user'
  passwordHash: String, // Hashed password
  lastLogin: Date,      // Last login timestamp
  status: String        // 'active', 'suspended', or 'deleted'
}
```

### 2. Product Model
```javascript
{
  name: String,               // Product name
  description: String,        // Product description
  version: String,            // Software version
  price: Number,              // Base price
  licenseTypes: [{            // Available license types
    type: String,             // License type name
    duration: Number,         // Duration in days
    price: Number             // Price for this license type
  }],
  features: [String],         // List of features
  systemRequirements: String, // System requirements
  downloadUrl: String,        // URL to download the software
  documentationUrl: String,   // URL to documentation
  licenseAgreement: String    // License agreement text
}
```

### 3. License Model
```javascript
{
  userId: ObjectId,       // Reference to User
  productId: ObjectId,    // Reference to Product
  licenseKey: String,     // Unique license key
  type: String,           // License type
  startDate: Date,        // When the license becomes active
  expiryDate: Date,       // When the license expires
  status: String,         // 'active', 'expired', or 'revoked'
  activations: [{         // List of device activations
    deviceId: String,     // Unique device identifier
    activatedAt: Date     // When the device was activated
  }],
  maxActivations: Number  // Maximum allowed activations
}
```

### 4. Purchase Model
```javascript
{
  userId: ObjectId,       // Reference to User
  productId: ObjectId,    // Reference to Product
  licenseType: String,    // Type of license purchased
  quantity: Number,       // Number of licenses
  amount: Number,         // Total purchase amount
  status: String,         // 'pending', 'approved', or 'rejected'
  approvedBy: ObjectId,   // Reference to User (admin who approved)
  approvedAt: Date        // When the purchase was approved
}
```

### 5. Analytics Model
```javascript
{
  type: String,           // 'product_view', 'license_activation', 'purchase'
  productId: ObjectId,    // Reference to Product
  userId: ObjectId,       // Reference to User
  timestamp: Date,        // When the event occurred
  metadata: Mixed         // Additional event data
}
```

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register a new user
- `POST /api/auth/login` - Login and get JWT token

### Users
- `GET /api/users` - Get all users (admin only)
- `GET /api/users/:id` - Get user by ID
- `PUT /api/users/:id` - Update user
- `DELETE /api/users/:id` - Delete user

### Products
- `POST /api/products` - Create a new product (admin only)
- `GET /api/products` - Get all products
- `GET /api/products/:id` - Get product by ID
- `PUT /api/products/:id` - Update product (admin only)
- `DELETE /api/products/:id` - Delete product (admin only)

### Licenses
- `POST /api/licenses` - Create a new license (admin only)
- `GET /api/licenses` - Get all licenses
- `GET /api/licenses/user` - Get licenses for current user
- `GET /api/licenses/:id` - Get license by ID
- `PUT /api/licenses/:id` - Update license (admin only)
- `DELETE /api/licenses/:id` - Delete license (admin only)
- `POST /api/licenses/activate/:licenseKey` - Activate a license on a device
- `POST /api/licenses/validate` - Validate a license

### Purchases
- `POST /api/purchases` - Create a new purchase
- `GET /api/purchases` - Get all purchases (admin only)
- `GET /api/purchases/user` - Get purchases for current user
- `GET /api/purchases/:id` - Get purchase by ID
- `PUT /api/purchases/:id/status` - Update purchase status (admin only)

### Analytics
- `GET /api/analytics/summary` - Get analytics summary
- `GET /api/analytics/products` - Get product analytics
- `GET /api/analytics/licenses` - Get license usage analytics

## Authentication & Authorization

### Authentication
- Uses JWT (JSON Web Tokens) for stateless authentication
- Tokens are issued upon successful login and must be included in the `Authorization` header for protected routes
- Token format: `Bearer <token>`

### Authorization
- Two main roles: `admin` and `user`
- Admin users have full access to all endpoints
- Regular users can only access and modify their own resources
- Role-based access control is implemented using middleware

### Middleware
1. **authenticateToken**: Verifies the JWT token and attaches the user to the request object
2. **authorizeRole**: Restricts access to users with specific roles

## Core Features

### 1. License Management
- Generate unique license keys
- Track license activations per device
- Validate license status
- Handle license expiration and revocation

### 2. Product Management
- Create and manage software products
- Define different license types and pricing
- Manage product versions and updates

### 3. User Management
- User registration and authentication
- Role-based access control
- User status management (active/suspended/deleted)

### 4. Purchase Workflow
- Submit purchase requests
- Admin approval process
- License generation upon approval
- Email notifications

### 5. Analytics & Reporting
- Track product views
- Monitor license activations
- Generate sales reports
- Usage statistics

## Setup & Configuration

### Prerequisites
- Node.js (v14 or higher)
- MongoDB (v4.4 or higher)
- npm or yarn

### Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd LMS_Backend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file in the root directory with the following variables:
   ```env
   PORT=5000
   MONGODB_URI=mongodb://localhost:27017/license_management
   JWT_SECRET=your_jwt_secret_key
   NODE_ENV=development
   ```

4. Start the development server:
   ```bash
   npm run dev
   ```

5. For production:
   ```bash
   npm start
   ```

## Error Handling

The API follows RESTful conventions for error responses:

- `400 Bad Request`: Invalid request data
- `401 Unauthorized`: Authentication required
- `403 Forbidden`: Insufficient permissions
- `404 Not Found`: Resource not found
- `500 Internal Server Error`: Server-side error

Example error response:
```json
{
  "message": "Error message",
  "error": "Detailed error information"
}
```

## Security Considerations

1. **Authentication**:
   - JWT tokens with expiration
   - Secure password hashing with bcrypt
   - Token-based authentication for API requests

2. **Authorization**:
   - Role-based access control
   - Principle of least privilege
   - Input validation

3. **Data Protection**:
   - Environment variables for sensitive data
   - No sensitive data in responses
   - Secure password storage

4. **API Security**:
   - CORS configuration
   - Rate limiting (recommended)
   - Input sanitization

5. **Dependencies**:
   - Regular dependency updates
   - Security audits

## Support

For support, please contact [nihaldm65@gmail.com]
