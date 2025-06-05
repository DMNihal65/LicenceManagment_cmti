# Software License Management System (LMS)

A comprehensive solution for managing software licenses, built with a modern tech stack including Node.js, React, and MongoDB. This system provides a complete solution for software vendors to manage their licenses, track activations, and handle user access.

## System Overview

The LMS consists of four main components:

1. **Backend (LMS_Backend)**: A Node.js/Express server that handles all business logic, authentication, and database operations
2. **Frontend (LMS_Frontend)**: A React-based web application for administrators and users to manage licenses
3. **Client (LMS_Client)**: A Node.js library that software vendors can integrate into their applications for license validation
4. **Simulation (LMS_Simulation)**: A testing environment to simulate license activation and validation

### Architecture

```
LMS_Backend (Node.js/Express) ←→ MongoDB
    ↑↓
LMS_Frontend (React)
    ↑↓
LMS_Client (Node.js Library)
    ↑↓
LMS_Simulation (Testing Environment)
```

## Features

- **User Management**
  - User registration and authentication
  - Role-based access control (Admin/User)
  - User profile management

- **License Management**
  - License key generation and validation
  - Machine-specific activation
  - License status tracking
  - License expiration handling

- **Product Management**
  - Product creation and management
  - Version control
  - Feature management
  - System requirements specification

- **Analytics**
  - Usage tracking
  - Activation statistics
  - Revenue monitoring
  - User activity logging

## Setup Instructions

### Prerequisites

- Node.js (v14 or higher)
- MongoDB
- npm or yarn

### Backend Setup (LMS_Backend)

1. Navigate to the backend directory:
   ```bash
   cd LMS_Backend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file with the following variables:
   ```
   PORT=5000
   MONGODB_URI=your_mongodb_connection_string
   JWT_SECRET=your_jwt_secret
   ```

4. Start the server:
   ```bash
   npm start
   ```

### Frontend Setup (LMS_Frontend)

1. Navigate to the frontend directory:
   ```bash
   cd LMS_Frontend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file:
   ```
   VITE_API_BASE_URL=http://localhost:5000
   ```

4. Start the development server:
   ```bash
   npm run dev
   ```

### Client Library Setup (LMS_Client)

1. Install the client library in your application:
   ```bash
   npm install ../LMS_Client
   ```

2. Import and use the LicenseManager:
   ```javascript
   const LicenseManager = require('lms-client');
   const licenseManager = new LicenseManager('http://your-api-url', 'path/to/license.txt');
   ```

### Simulation Setup (LMS_Simulation)

1. Navigate to the simulation directory:
   ```bash
   cd LMS_Simulation
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Update the API_URL in `activation.js` to point to your backend server

4. Run the simulation:
   ```bash
   node activation.js
   ```

## Core Components

### Backend (LMS_Backend)

Key files and their purposes:

- `src/server.js`: Main server setup and route configuration
- `src/routes/`: API route definitions
  - `auth.js`: Authentication routes
  - `licenseRoutes.js`: License management routes
  - `productRoutes.js`: Product management routes
  - `userRoutes.js`: User management routes
- `src/controllers/`: Business logic implementation
- `src/middleware/`: Custom middleware (auth, validation)
- `src/models/`: MongoDB schema definitions

### Frontend (LMS_Frontend)

Key components:

- `src/App.jsx`: Main application component with routing
- `src/components/`: React components
  - `AdminDashboard.jsx`: Admin control panel
  - `UserDashboard.jsx`: User dashboard
  - `SignIn.jsx`: Authentication
  - `Layout.jsx`: Common layout wrapper
  - Various management components for licenses, products, and users

### Client Library (LMS_Client)

The `LicenseManager` class provides:

- License activation
- License validation
- Machine ID generation
- Local license storage

### Simulation (LMS_Simulation)

Provides testing capabilities:

- License activation simulation
- Regular validation checks
- Error handling simulation

## API Documentation

### Authentication Endpoints

- `POST /api/auth/register`: Register new user
- `POST /api/auth/login`: User login

### License Endpoints

- `POST /api/licenses/activate/:licenseKey`: Activate license
- `POST /api/licenses/validate`: Validate license
- `GET /api/licenses/user`: Get user's licenses
- `GET /api/licenses`: Get all licenses (admin)

### Product Endpoints

- `GET /api/products`: List all products
- `POST /api/products`: Create product (admin)
- `PUT /api/products/:id`: Update product (admin)
- `DELETE /api/products/:id`: Delete product (admin)

### User Endpoints

- `GET /api/users`: List all users (admin)
- `GET /api/users/:id`: Get user details
- `PUT /api/users/:id`: Update user
- `DELETE /api/users/:id`: Delete user (admin)

## Security Features

- JWT-based authentication
- Role-based access control
- Secure password hashing
- Machine-specific license binding
- API request validation
- CORS protection

## Development Guidelines

1. **Code Style**
   - Follow ESLint configuration
   - Use meaningful variable names
   - Add comments for complex logic

2. **Testing**
   - Use the simulation environment for testing
   - Test all license scenarios
   - Verify security measures

3. **Deployment**
   - Set up proper environment variables
   - Configure CORS for production
   - Use secure MongoDB connection
   - Implement proper logging

## Troubleshooting

Common issues and solutions:

1. **License Activation Fails**
   - Verify machine ID generation
   - Check license key validity
   - Ensure backend connectivity

2. **Authentication Issues**
   - Verify JWT token
   - Check user credentials
   - Validate role permissions

3. **Database Connection**
   - Verify MongoDB connection string
   - Check network connectivity
   - Validate database permissions

## Support

For support, please contact [nihaldm65@gmail.com]
