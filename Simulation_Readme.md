# License Management System - Simulation Documentation

## Table of Contents
1. [Overview](#overview)
2. [Purpose](#purpose)
3. [Architecture](#architecture)
4. [Components](#components)
5. [Key Features](#key-features)
6. [Installation](#installation)
7. [Configuration](#configuration)
8. [Usage](#usage)
9. [Workflow](#workflow)
10. [Error Handling](#error-handling)
11. [Security Considerations](#security-considerations)
12. [Troubleshooting](#troubleshooting)
13. [Support](#support)

## Overview

The License Management System (LMS) Simulation is a Node.js application that demonstrates how to integrate and test the license management functionality in a client application. It simulates the license activation and validation process that would be used in a production environment.

## Purpose

The simulation serves several key purposes:
- Demonstrate the end-to-end license activation flow
- Test license validation mechanisms
- Verify machine binding functionality
- Provide a reference implementation for client integration
- Facilitate testing and debugging of the license management system

## Architecture

The simulation follows a simple client-server architecture:

```
+----------------+     +------------------+     +-----------------+
|                |     |                  |     |                 |
|  Client        |<--->|  LMS Simulation  |<--->|  LMS Backend    |
|  Application   |     |  (Node.js)       |     |  (REST API)     |
|                |     |                  |     |                 |
+----------------+     +------------------+     +-----------------+
```

## Components

### 1. LicenseManager
- Handles all license-related operations
- Manages license activation and validation
- Implements machine binding
- Handles local license storage

### 2. Activation Module
- Manages the license activation process
- Handles communication with the license server
- Manages local license file storage

### 3. Validation Module
- Periodically validates the license
- Enforces license terms and conditions
- Handles license expiration and revocation

## Key Features

1. **License Activation**
   - One-time activation with unique license key
   - Machine binding using hardware fingerprinting
   - Network-based activation with fallback options

2. **License Validation**
   - Periodic license validation
   - Offline validation support
   - Tamper detection

3. **Error Handling**
   - Comprehensive error messages
   - Graceful degradation
   - Recovery mechanisms

4. **Security**
   - Secure license storage
   - Tamper detection
   - Secure communication with license server

## Installation

### Prerequisites
- Node.js (v14 or higher)
- npm (v6 or higher)
- Access to the LMS backend API

### Steps

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd LMS_Simulation
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

## Configuration

### Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
API_URL=http://your-backend-url/api/licenses
LICENSE_FILE_PATH=./license.txt
LICENSE_KEY=your-license-key
```

### Configuration Options

| Variable | Description | Default |
|----------|-------------|---------|
| `API_URL` | Base URL of the license management API | `http://localhost:5000/api/licenses` |
| `LICENSE_FILE_PATH` | Path to store the license file | `./license.txt` |
| `LICENSE_KEY` | Your license key | (required) |

## Usage

### Basic Usage

```bash
node activation.js
```

### Command Line Options

```bash
node activation.js --key=YOUR_LICENSE_KEY --api=http://your-backend-url
```

### Programmatic Usage

```javascript
const LicenseManager = require('./LMS_Client/licenseManager');
const licenseManager = new LicenseManager(API_URL, LICENSE_FILE_PATH);

// Activate license
async function activate() {
  try {
    const result = await licenseManager.activateLicense(LICENSE_KEY);
    console.log('Activation successful:', result);
  } catch (error) {
    console.error('Activation failed:', error.message);
  }
}

// Verify license
async function verify() {
  try {
    const isValid = await licenseManager.verifyLicense();
    console.log('License is', isValid ? 'valid' : 'invalid');
  } catch (error) {
    console.error('Verification failed:', error.message);
  }
}
```

## Workflow

### 1. First Run (Activation)
1. Application starts
2. Checks for existing valid license
3. If no valid license found, prompts for activation
4. Sends activation request to license server
5. Stores activated license locally
6. Starts application with full functionality

### 2. Subsequent Runs (Validation)
1. Application starts
2. Loads stored license
3. Validates license locally
4. Optionally validates with server (if online)
5. Starts application if license is valid

### 3. Periodic Validation
1. Application runs periodic validation (e.g., every 30 seconds)
2. Checks for license expiration
3. Verifies machine binding
4. Handles license revocation

## Error Handling

### Common Errors

| Error Code | Description | Resolution |
|------------|-------------|------------|
| `LICENSE_INVALID` | License is invalid or corrupted | Reactivate license |
| `LICENSE_EXPIRED` | License has expired | Renew license |
| `MACHINE_MISMATCH` | Running on unauthorized device | Deactivate old device first |
| `NETWORK_ERROR` | Cannot reach license server | Check connection |

### Error Recovery

1. **Network Issues**
   - Retry with exponential backoff
   - Use cached validation if available
   - Graceful degradation of features

2. **Invalid License**
   - Clear local license storage
   - Prompt for reactivation
   - Provide error details to user

## Security Considerations

1. **License Storage**
   - Store license data securely
   - Encrypt sensitive information
   - Use OS-specific secure storage when available

2. **Network Security**
   - Always use HTTPS
   - Verify server certificates
   - Implement request signing

3. **Tamper Protection**
   - Code obfuscation
   - Integrity checks
   - Anti-debugging measures

## Troubleshooting

### Common Issues

1. **Activation Fails**
   - Verify internet connection
   - Check API URL and license key
   - Ensure server is running and accessible

2. **Validation Fails**
   - Check system time and date
   - Verify license file permissions
   - Check for hardware changes

3. **Performance Issues**
   - Reduce validation frequency
   - Optimize network requests
   - Implement caching

## Support

For support, please contact [nihaldm65@gmail.com]
