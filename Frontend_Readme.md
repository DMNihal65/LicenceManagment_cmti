# License Management System - Frontend Documentation

## Table of Contents
1. [Overview](#overview)
2. [Tech Stack](#tech-stack)
3. [Project Structure](#project-structure)
4. [Features](#features)
5. [Component Architecture](#component-architecture)
6. [Routing](#routing)
7. [State Management](#state-management)
8. [API Integration](#api-integration)
9. [Authentication Flow](#authentication-flow)
10. [UI Components](#ui-components)
11. [Responsive Design](#responsive-design)
12. [Environment Variables](#environment-variables)
13. [Setup & Installation](#setup--installation)
14. [Development Workflow](#development-workflow)
15. [Testing](#testing)
16. [Deployment](#deployment)
17. [Troubleshooting](#troubleshooting)
18. [Support](#support)

## Overview

The License Management System (LMS) Frontend is a modern, responsive web application built with React and Ant Design. It provides an intuitive interface for both administrators and end-users to manage software licenses, products, and user accounts.

## Tech Stack

- **Framework**: React 18
- **UI Library**: Ant Design v5
- **Routing**: React Router v6
- **HTTP Client**: Axios
- **Styling**: Tailwind CSS
- **Animation**: Framer Motion, React Spring
- **Build Tool**: Vite
- **Package Manager**: npm

## Project Structure

```
src/
├── assets/            # Static assets (images, fonts, etc.)
├── components/        # Reusable UI components
│   ├── Admin/        # Admin-specific components
│   ├── User/         # User-specific components
│   ├── Common/       # Shared components
│   ├── Layout.jsx    # Main layout component
│   └── ...
├── hooks/            # Custom React hooks
├── lib/              # Utility functions and configurations
├── App.jsx           # Main application component
└── main.jsx          # Application entry point
```

## Features

### Admin Features
- Dashboard with key metrics and analytics
- User management (CRUD operations)
- Product management
- License generation and management
- Purchase approval workflow
- Activity logs

### User Features
- Product catalog
- License management
- Purchase history
- Profile management
- License activation

## Component Architecture

### Key Components

1. **Layout**
   - Handles the overall page structure
   - Includes navigation, header, and footer
   - Conditionally renders sidebar based on user role

2. **ProtectedRoute**
   - Guards routes based on authentication status
   - Implements role-based access control
   - Redirects unauthenticated users to login

3. **Admin Components**
   - `AdminDashboard`: Overview of system metrics
   - `AdminUsers`: User management interface
   - `AdminProducts`: Product management
   - `AdminLicenses`: License administration
   - `AdminPurchases`: Purchase approval workflow

4. **User Components**
   - `UserDashboard`: User overview
   - `UserProducts`: Product catalog
   - `UserLicenses`: License management
   - `Profile`: User profile and settings

## Routing

### Public Routes
- `/` - Landing page
- `/signin` - User authentication
- `/signup` - New user registration

### Protected Routes (Admin)
- `/admin/dashboard` - Admin dashboard
- `/admin/users` - User management
- `/admin/products` - Product management
- `/admin/licenses` - License management
- `/admin/purchases` - Purchase approvals

### Protected Routes (User)
- `/user/dashboard` - User dashboard
- `/user/products` - Product catalog
- `/user/licenses` - User's licenses
- `/profile` - User profile

## State Management

The application uses React's built-in state management with the Context API for global state. Key state management patterns include:

1. **Local State**
   - Component-level state using `useState`
   - Form state management with Ant Design's `Form` component

2. **Global State**
   - Authentication state (user info, token)
   - Application theme
   - Global loading states

3. **Data Fetching**
   - `useEffect` for data fetching
   - Loading and error states
   - Data caching strategies

## API Integration

The frontend communicates with the backend REST API using Axios. Key integration points include:

### Authentication
```javascript
// Login
export const login = async (credentials) => {
  const response = await axios.post('/api/auth/login', credentials);
  return response.data;
};

// Get current user
export const getCurrentUser = async () => {
  const response = await axios.get('/api/auth/me', {
    headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
  });
  return response.data;
};
```

### Products
```javascript
// Get all products
export const getProducts = async () => {
  const response = await axios.get('/api/products');
  return response.data;
};

// Create product
export const createProduct = async (productData) => {
  const response = await axios.post('/api/products', productData, {
    headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
  });
  return response.data;
};
```

### Error Handling
```javascript
// Axios interceptor for handling errors
axios.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      // Handle unauthorized
      localStorage.removeItem('token');
      window.location.href = '/signin';
    }
    return Promise.reject(error);
  }
);
```

## Authentication Flow

1. **Login Process**
   - User submits credentials
   - Server validates and returns JWT token
   - Token stored in localStorage
   - User redirected to appropriate dashboard

2. **Protected Routes**
   - `ProtectedRoute` checks for valid token
   - Redirects to login if not authenticated
   - Validates user role for admin routes

3. **Token Management**
   - Token stored in localStorage
   - Included in Authorization header for API requests
   - Automatic token refresh (if implemented)

## UI Components

The application uses Ant Design components with custom styling. Key UI patterns include:

1. **Forms**
   - Form validation
   - Form layout
   - Error handling

2. **Tables**
   - Sorting and filtering
   - Pagination
   - Row selection

3. **Modals**
   - CRUD operations
   - Confirmation dialogs

4. **Navigation**
   - Responsive sidebar
   - Breadcrumbs
   - Tabs

## Responsive Design

The application is fully responsive and works on all device sizes:

- Mobile-first approach
- Responsive grid system
- Collapsible sidebar
- Adaptive forms and tables

## Environment Variables

Create a `.env` file in the root directory:

```env
VITE_API_BASE_URL=http://localhost:5000
# Other environment variables...
```

## Setup & Installation

1. **Prerequisites**
   - Node.js (v16+)
   - npm (v8+)
   - Backend server running

2. **Install Dependencies**
   ```bash
   npm install
   ```

3. **Configure Environment**
   Copy `.env.example` to `.env` and update the values

4. **Start Development Server**
   ```bash
   npm run dev
   ```

5. **Build for Production**
   ```bash
   npm run build
   ```

## Development Workflow

1. **Branching Strategy**
   - `main` - Production-ready code
   - `develop` - Development branch
   - `feature/` - New features
   - `bugfix/` - Bug fixes
   - `hotfix/` - Critical production fixes

2. **Code Style**
   - ESLint for code linting
   - Prettier for code formatting
   - Follow React best practices

## Testing

### Unit Testing
```bash
npm test
```

### E2E Testing
```bash
npm run test:e2e
```

### Testing Libraries
- Jest
- React Testing Library
- Cypress (for E2E)

## Deployment

### Production Build
```bash
npm run build
```

### Deployment Options
1. **Static Hosting**
   - Vercel
   - Netlify
   - GitHub Pages

2. **Docker**
   ```dockerfile
   # Dockerfile
   FROM node:16-alpine as build
   WORKDIR /app
   COPY package*.json ./
   RUN npm install
   COPY . .
   RUN npm run build

   FROM nginx:alpine
   COPY --from=build /app/dist /usr/share/nginx/html
   EXPOSE 80
   CMD ["nginx", "-g", "daemon off;"]
   ```

## Troubleshooting

### Common Issues

1. **CORS Errors**
   - Ensure backend has proper CORS configuration
   - Check API base URL

2. **Authentication Issues**
   - Verify token is being set correctly
   - Check token expiration
   - Validate JWT secret

3. **Build Failures**
   - Check Node.js version
   - Clear npm cache
   - Delete node_modules and reinstall

## Support

For support, please contact [nihaldm65@gmail.com]
