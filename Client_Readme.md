# License Management System - Client Documentation

## Table of Contents
1. [Overview](#overview)
2. [Features](#features)
3. [Installation](#installation)
4. [Usage](#usage)
5. [API Reference](#api-reference)
6. [Error Handling](#error-handling)
7. [Security Considerations](#security-considerations)
8. [Troubleshooting](#troubleshooting)
9. [Support](#support)

## Overview

The License Management System (LMS) Client is a Node.js module that provides a simple interface for managing software licenses in client applications. It handles license activation, validation, and machine binding to prevent unauthorized use of licensed software.

## Features

- **License Activation**: Activate a license key on the current machine
- **License Validation**: Verify the validity of a previously activated license
- **Machine Binding**: Bind licenses to specific machines using hardware fingerprints
- **Persistent Storage**: Store license information locally for offline validation
- **Error Handling**: Comprehensive error handling and logging

## Installation

1. Install the package using npm:
   ```bash
   npm install @your-org/license-manager
   ```

2. Import the LicenseManager in your application:
   ```javascript
   const LicenseManager = require('@your-org/license-manager');
   ```

## Usage

### Initialization

```javascript
const LicenseManager = require('./licenseManager');
const path = require('path');

// Initialize with your API URL and license file path
const licenseManager = new LicenseManager(
  'https://your-api-url.com/api/licenses',
  path.join(__dirname, 'license.json')
);
```

### Activating a License

```javascript
async function activate(licenseKey) {
  try {
    const result = await licenseManager.activateLicense(licenseKey);
    console.log('License activated successfully:', result);
    return true;
  } catch (error) {
    console.error('Activation failed:', error.message);
    return false;
  }
}
```

### Verifying a License

```javascript
async function checkLicense() {
  try {
    const isValid = await licenseManager.verifyLicense();
    if (isValid) {
      console.log('License is valid');
      return true;
    } else {
      console.log('License is invalid or expired');
      return false;
    }
  } catch (error) {
    console.error('License verification failed:', error.message);
    return false;
  }
}
```

### Complete Example

```javascript
const LicenseManager = require('./licenseManager');
const path = require('path');

async function main() {
  const licenseManager = new LicenseManager(
    'https://your-api-url.com/api/licenses',
    path.join(__dirname, 'license.json')
  );

  // Check if license is already active
  const isValid = await licenseManager.verifyLicense();
  
  if (!isValid) {
    // If not valid, prompt for license key
    const licenseKey = prompt('Please enter your license key:');
    if (licenseKey) {
      await licenseManager.activateLicense(licenseKey);
    } else {
      console.error('License key is required');
      process.exit(1);
    }
  }

  // Continue with your application logic
  console.log('Application started with valid license');
}

main().catch(console.error);
```

## API Reference

### `new LicenseManager(apiUrl, licenseFilePath)`

Creates a new instance of LicenseManager.

**Parameters:**
- `apiUrl` (String): Base URL of your license management API
- `licenseFilePath` (String): Path where the license file will be stored

### `activateLicense(licenseKey)`

Activates a license on the current machine.

**Parameters:**
- `licenseKey` (String): The license key to activate

**Returns:** Promise that resolves with the activation response or rejects with an error

### `verifyLicense()`

Verifies if the current machine has a valid license.

**Returns:** Promise that resolves to `true` if the license is valid, `false` otherwise

### `getMachineId()`

Generates a unique machine identifier based on hardware and network configuration.

**Returns:** String containing a SHA-256 hash of machine-specific information

## Error Handling

The client provides detailed error messages for common issues:

- **Network Errors**: When unable to reach the license server
- **Invalid License**: When the provided license key is invalid or expired
- **Activation Limit Reached**: When the license has reached its maximum activation limit
- **Machine Mismatch**: When verifying on a different machine than the one used for activation

Example error handling:

```javascript
try {
  await licenseManager.activateLicense('YOUR-LICENSE-KEY');
} catch (error) {
  if (error.response) {
    // Server responded with an error status code
    console.error('Server error:', error.response.data.message);
  } else if (error.request) {
    // Request was made but no response received
    console.error('No response from server. Please check your internet connection.');
  } else {
    // Something else went wrong
    console.error('Error:', error.message);
  }
}
```

## Security Considerations

1. **Machine Identification**
   - Uses a combination of CPU, memory, and network interface details
   - Generates a consistent but non-reversible machine identifier
   - Prevents simple spoofing of machine identity

2. **License Storage**
   - Stores license information in a local JSON file
   - File permissions should be set to restrict access to authorized users only
   - Consider encrypting sensitive data if storing additional information

3. **Network Security**
   - Always use HTTPS for API communication
   - Implement rate limiting on the server side
   - Validate all server responses before processing

4. **Anti-Tampering**
   - The client includes basic checks but should be combined with additional obfuscation
   - Consider using tools like `bytenode` to compile critical code

## Troubleshooting

### Common Issues

1. **Activation Fails**
   - Verify internet connection
   - Check that the API URL is correct
   - Ensure the license key is valid and has available activations

2. **Verification Fails**
   - The license file may be corrupt - try deleting it and reactivating
   - System hardware changes may cause verification to fail
   - Check if the license has expired

3. **Network Errors**
   - Verify the API endpoint is accessible
   - Check for firewall or proxy issues
   - Ensure CORS is properly configured on the server

## Support

For support, please contact [nihaldm65@gmail.com]
