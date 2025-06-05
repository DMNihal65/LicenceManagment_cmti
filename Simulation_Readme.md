# License Management System - Simulation

## Table of Contents
1. [Quick Start](#quick-start)
2. [Overview](#overview)
3. [Prerequisites](#prerequisites)
4. [Installation](#installation)
5. [Configuration](#configuration)
6. [Running the Simulation](#running-the-simulation)
7. [Understanding the Output](#understanding-the-output)
8. [Troubleshooting](#troubleshooting)
9. [FAQ](#faq)

## Quick Start

1. Ensure you have Node.js installed (v14+)
2. Clone the repository
3. Navigate to the LMS_Simulation directory
4. Run: `node activation.js`

## Overview

This simulation demonstrates the license activation and validation flow of the License Management System. It includes:

- License activation with machine binding
- Local license validation
- Periodic license checks
- Error handling and logging

## Prerequisites

- Node.js v14 or higher
- A valid license key from the LMS backend
- Network access to the LMS backend API (default: http://172.18.7.89:5000)

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

1. Navigate to the project directory:
   ```bash
   cd LMS_Simulation
   ```

2. Install the required dependencies:
   ```bash
   npm install
   ```

## Configuration

### 1. Update License Key
Edit `activation.js` and set your license key:
```javascript
const LICENSE_KEY = 'your-license-key-here';
```

### 2. Verify API URL
Ensure the API URL in `activation.js` points to your LMS backend:
```javascript
const API_URL = 'http://172.18.7.89:5000/api/licenses';
```

### 3. License File Location
By default, the license file is stored as `license.txt` in the simulation directory.

## Running the Simulation

### First Run (Activation)
1. Ensure you have a valid license key
2. Run the simulation:
   ```bash
   node activation.js
   ```
3. The script will:
   - Generate a machine ID
   - Attempt to activate the license
   - Save the activation details to `license.txt`

### Subsequent Runs
- The simulation will automatically validate the existing license
- No activation is needed unless the machine changes

## Understanding the Output

### Successful Activation
```
🚀 Starting license activation process...
Checking for existing license...
No existing license file found, attempting to activate...
Attempting to activate license: your-license-key
Entering activateLicense function
Request body: { licenseKey: 'your-license-key', machineId: '...' }
License activation response: { message: 'License activated successfully', ... }
✅ License activated successfully. Access granted to the application.
🚀 Application started successfully!
```

### Already Activated
```
🚀 Starting license activation process...
Checking for existing license...
License file found, verifying...
✅ License is already activated and valid. Access granted to the application.
🚀 Application started successfully!
```

### Error Cases
- **Invalid License**: `❌ Failed to activate the license: Invalid license key`
- **Network Issues**: `❌ Failed to activate the license: Network Error`
- **Server Error**: `❌ Failed to activate the license: Internal Server Error`

## Troubleshooting

### Common Issues

1. **Machine ID Mismatch**
   - Error: `License already activated on this machine`
   - Solution: Deactivate the license from the previous machine or use a different license key

2. **Network Errors**
   - Error: `Network Error` or `ECONNREFUSED`
   - Solution: Verify the API URL and ensure the backend is running

3. **Invalid License**
   - Error: `Invalid license key`
   - Solution: Double-check the license key and ensure it's active

4. **Permission Issues**
   - Error: `EACCES` when writing license file
   - Solution: Ensure the application has write permissions to the directory

## FAQ

### Q: How do I get a new license key?
A: Contact your system administrator to generate a new license key.

### Q: What happens if I change my hardware?
A: The license is tied to the machine ID. You'll need to deactivate the license from the old machine first.

### Q: How do I check if my license is valid?
A: The application performs periodic validations. You can also check the `license.txt` file for activation details.

### Q: Can I run this on multiple machines?
A: Each machine requires its own license key unless your license allows multiple activations.

### Q: How do I completely reset the activation?
A: Delete the `license.txt` file and restart the application to trigger a new activation.

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
