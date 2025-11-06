<p align="center">
  <img src="https://customer-assets.emergentagent.com/job_api-legal-hub/artifacts/1fjsrc03_logo-4X.png" alt="Utility Server Logo" width="200"/>
</p>

# TTS Utility Server API Documentation

## Version: 4.21.04
**Last Updated:** November 6, 2025

---

## Table of Contents

1. [Overview](#overview)
2. [Getting Started](#getting-started)
3. [Authentication](#authentication)
4. [Base URLs](#base-urls)
5. [Common Headers](#common-headers)
6. [API Endpoints](#api-endpoints)
   - [Authentication & Session Management](#authentication--session-management)
   - [User Management](#user-management)
   - [Key Management](#key-management)
   - [Logging & Monitoring](#logging--monitoring)
   - [Statistics](#statistics)
   - [Spider Module](#spider-module)
   - [Sitemap Management](#sitemap-management)
7. [Request/Response Formats](#requestresponse-formats)
8. [Error Codes](#error-codes)
9. [Rate Limiting](#rate-limiting)
10. [Best Practices](#best-practices)

---

## Overview

The TTS Utility Server API provides a comprehensive suite of endpoints for managing customer authentication, session management, logging, statistics, and cache operations. This RESTful API supports both `application/x-www-form-urlencoded` and `application/json` content types.

### Key Features

- **Authentication**: Secure member login and session management
- **User Management**: Retrieve and manage user information
- **Key Management**: Rotate and update access keys and secret keys
- **Logging**: Real-time log monitoring and historical log retrieval
- **Statistics**: Fetch usage statistics by date or month
- **Spider Module**: Advanced caching and content delivery operations
- **Sitemap Management**: Add and update XML sitemaps

---

## Getting Started

### Prerequisites

To use the API, you need:
- A valid `identifire` (client identifier)
- An `x-tts-cc-access-key` (for authenticated requests)
- A `secret` key (optional, depending on endpoint)
- Client version identifier (e.g., `V4.21.04`)

### Quick Start Example

```bash
curl -X POST "https://prod.services.utility-server.com/your-identifier/customers/member_login" \
  -H "Content-Type: application/json" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -d '{
    "email_id": "user@example.com",
    "password": "your_password",
    "identifire": "your-identifier",
    "client_ver": "V4.21.04",
    "ip_address": "192.168.1.1",
    "mac_address": "-",
    "checksum": "generated_checksum_value",
    "secret": "YOUR_SECRET_KEY"
  }'
```

---

## Authentication

The API uses a combination of authentication methods:

### 1. Access Key Authentication
Include your access key in the request header:
```
x-tts-cc-access-key: YOUR_ACCESS_KEY
```

### 2. Session-Based Authentication
After successful login, include session information:
```
session-id: YOUR_SESSION_ID
member-email: user@example.com
```

### 3. Checksum Validation
For JSON requests, include a checksum for data integrity verification.

---

## Base URLs

### Production Environment
```
https://prod.services.utility-server.com/{identifire}
```

### Development Environment
```
http://btv.localhost:3000/{identifire}
```

### Spider Module (Production)
```
https://spider.services.utility-server.com
```

**Note:** Replace `{identifire}` with your client identifier (e.g., `www.example.com`, `preprod.example.com`)

---

## Common Headers

All API requests should include the following headers:

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `Content-Type` | String | Yes | `application/json` or `application/x-www-form-urlencoded` |
| `User-Agent` | String | Yes | Format: `TTS-Utility-Client/{CLIENT_ID}/{ENV}/{VERSION}` |
| `x-tts-cc-access-key` | String | Yes* | Your API access key |
| `client_ver` | String | Yes | API version (e.g., `V4.21.04`) |
| `session-id` | String | No** | Session identifier (required for authenticated endpoints) |
| `member-email` | String | No** | Member email (required for authenticated endpoints) |
| `x-tts-module` | String | No | Module identifier (e.g., `SPIDER`) |
| `x-target-force` | String | No | Force target environment (e.g., `testing`) |
| `x-log-enabled` | String | No | Enable specific log levels (e.g., `INFO,DEBUG,VIGI,WARN,ERR`) |
| `x-no-cache` | String | No | Bypass cache (value: `TRUE`) |

\* May be empty for certain endpoints  
\** Required for authenticated endpoints after login

---

## API Endpoints

## Authentication & Session Management

### 1. Member Login

Authenticate a user and create a new session.

**Endpoint:** `POST /{identifire}/customers/member_login`

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `email_id` | String | Yes | User's email address |
| `password` | String | Yes | User's password |
| `identifire` | String | Yes | Client identifier |
| `secret` | String | No | Secret key for additional security |
| `client_ver` | String | Yes | API version |
| `checksum` | String | Yes* | Data integrity checksum |
| `ip_address` | String | Yes* | Client IP address |
| `mac_address` | String | Yes* | Client MAC address (use `-` if unavailable) |
| `action` | String | No | Optional action parameter |

\* Required when using JSON format

**Example Request (JSON):**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com/customers/member_login" \
  -H "Content-Type: application/json" \
  -H "User-Agent: TTS-Utility-Client/505.EXAMPLE.PROD/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "x-tts-module: SPIDER" \
  -d '{
    "email_id": "user@example.com",
    "password": "secure_password",
    "identifire": "www.example.com",
    "secret": "YOUR_SECRET_KEY",
    "client_ver": "V4.21.04",
    "checksum": "generated_checksum_hash",
    "ip_address": "192.168.1.100",
    "mac_address": "-"
  }'
```

**Example Request (Form Data):**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com/customers/member_login" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  --data-urlencode "email_id=user@example.com" \
  --data-urlencode "password=secure_password" \
  --data-urlencode "identifire=www.example.com" \
  --data-urlencode "secret=YOUR_SECRET_KEY" \
  --data-urlencode "client_ver=V4.21.04"
```

**Success Response:**

```json
{
  "session_id": "zO4WlLQLT8fj4rakFbnvX~u92nq4A-fO",
  "user_type": "client",
  "message": "Logged-in successful, inactive session time-out is 43200 minutes."
}
```

**Error Response:**

```
Active login session found, please logout existing session, else you can force terminate all active sessions.
```

---

### 2. Reset Password

Reset a user's password. A new password will be emailed to the registered email address.

**Endpoint:** `POST /{identifire}/customers/reset_password`

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `email_id` | String | Yes | User's email address |
| `identifire` | String | Yes | Client identifier |
| `secret` | String | No | Secret key |
| `client_ver` | String | Yes | API version |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com/customers/reset_password" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  --data-urlencode "email_id=user@example.com" \
  --data-urlencode "identifire=www.example.com" \
  --data-urlencode "client_ver=V4.21.04"
```

**Success Response:**

```
Password reset successfully, and new password has been emailed to your registered email id.
```

---

### 3. Terminate Session

Forcefully terminate an active user session.

**Endpoint:** `POST /{identifire}/customers/terminate_session`

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `email_id` | String | Yes | User's email address |
| `password` | String | Yes | User's password |
| `identifire` | String | Yes | Client identifier |
| `secret` | String | No | Secret key |
| `client_ver` | String | Yes | API version |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com/customers/terminate_session" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  --data-urlencode "email_id=user@example.com" \
  --data-urlencode "password=secure_password" \
  --data-urlencode "identifire=www.example.com" \
  --data-urlencode "client_ver=V4.21.04"
```

**Success Response:**

```
Terminated active session, please relogin to continue.
```

---

### 4. Member Logout

Logout the current user session.

**Endpoint:** `POST /{identifire}/customers/member_logout`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `identifire` | String | Yes | Client identifier |
| `secret` | String | No | Secret key |
| `client_ver` | String | Yes | API version |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com/customers/member_logout" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "session-id: zO4WlLQLT8fj4rakFbnvX~u92nq4A-fO" \
  -H "member-email: user@example.com" \
  --data-urlencode "identifire=www.example.com" \
  --data-urlencode "client_ver=V4.21.04"
```

**Success Response:**

```
Logged-out active session, please relogin to continue.
```

---

## User Management

### 5. Get Users

Retrieve user information for the authenticated session.

**Endpoint:** `POST /{identifire}/customers/get_users`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `identifire` | String | Yes | Client identifier |
| `secret` | String | No | Secret key |
| `client_ver` | String | Yes | API version |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com/customers/get_users" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "session-id: zO4WlLQLT8fj4rakFbnvX~u92nq4A-fO" \
  -H "member-email: user@example.com" \
  --data-urlencode "identifire=www.example.com" \
  --data-urlencode "client_ver=V4.21.04"
```

**Success Response:**

Returns user information in JSON format.

---

## Key Management

### 6. Rotate Access Key

Generate a new access key and invalidate the current one.

**Endpoint:** `POST /{identifire}/customers/rotate_access_key`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email
- `x-tts-module`: Set to `SPIDER`

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `identifire` | String | Yes | Client identifier |
| `client_ver` | String | Yes | API version |
| `checksum` | String | Yes | Data integrity checksum |
| `ip_address` | String | Yes | Client IP address |
| `mac_address` | String | Yes | Client MAC address |
| `secret` | String | No | Secret key |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com/customers/rotate_access_key" \
  -H "Content-Type: application/json" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-module: SPIDER" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  -d '{
    "identifire": "www.example.com",
    "client_ver": "V4.21.04",
    "checksum": "generated_checksum_hash",
    "ip_address": "192.168.1.100",
    "mac_address": "-",
    "secret": ""
  }'
```

**Success Response:**

Returns the new access key.

---

### 7. Push Access Key

Update to a specific access key.

**Endpoint:** `POST /{identifire}/customers/push_access_key`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email
- `x-tts-module`: Set to `SPIDER`

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `identifire` | String | Yes | Client identifier |
| `client_ver` | String | Yes | API version |
| `checksum` | String | Yes | Data integrity checksum |
| `ip_address` | String | Yes | Client IP address |
| `mac_address` | String | Yes | Client MAC address |
| `new_access_key` | String | Yes | New access key to set |
| `secret` | String | No | Secret key |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com/customers/push_access_key" \
  -H "Content-Type: application/json" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-module: SPIDER" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  -d '{
    "identifire": "www.example.com",
    "client_ver": "V4.21.04",
    "checksum": "generated_checksum_hash",
    "ip_address": "192.168.1.100",
    "mac_address": "-",
    "new_access_key": "NEW_ACCESS_KEY_VALUE",
    "secret": ""
  }'
```

**Success Response:**

Confirms the access key has been updated.

---

### 8. Rotate Secret Key

Generate a new secret key and invalidate the current one.

**Endpoint:** `POST /{identifire}/customers/rotate_secret_key`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email
- `x-tts-module`: Set to `SPIDER`
- `x-tts-cc-access-key`: Current valid access key

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `identifire` | String | Yes | Client identifier |
| `client_ver` | String | Yes | API version |
| `checksum` | String | Yes | Data integrity checksum |
| `ip_address` | String | Yes | Client IP address |
| `mac_address` | String | Yes | Client MAC address |
| `secret` | String | No | Current secret key |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com/customers/rotate_secret_key" \
  -H "Content-Type: application/json" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_CURRENT_ACCESS_KEY" \
  -H "x-tts-module: SPIDER" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  -d '{
    "identifire": "www.example.com",
    "client_ver": "V4.21.04",
    "checksum": "generated_checksum_hash",
    "ip_address": "192.168.1.100",
    "mac_address": "-",
    "secret": ""
  }'
```

**Success Response:**

Returns the new secret key.

---

### 9. Push Secret Key

Update to a specific secret key.

**Endpoint:** `POST /{identifire}/customers/push_secret_key`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email
- `x-tts-module`: Set to `SPIDER`
- `x-tts-cc-access-key`: Current valid access key

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `identifire` | String | Yes | Client identifier |
| `client_ver` | String | Yes | API version |
| `checksum` | String | Yes | Data integrity checksum |
| `ip_address` | String | Yes | Client IP address |
| `mac_address` | String | Yes | Client MAC address |
| `new_secret_key` | String | Yes | New secret key to set |
| `secret` | String | No | Current secret key |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com/customers/push_secret_key" \
  -H "Content-Type: application/json" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_CURRENT_ACCESS_KEY" \
  -H "x-tts-module: SPIDER" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  -d '{
    "identifire": "www.example.com",
    "client_ver": "V4.21.04",
    "checksum": "generated_checksum_hash",
    "ip_address": "192.168.1.100",
    "mac_address": "-",
    "new_secret_key": "NEW_SECRET_KEY_VALUE",
    "secret": ""
  }'
```

**Success Response:**

Confirms the secret key has been updated.

---

## Logging & Monitoring

### 10. Live Logs

Retrieve real-time logs with optional filtering.

**Endpoint:** `POST /{identifire}`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email
- `x-tts-module`: Set to `SPIDER`

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `action` | String | Yes | Set to `liveLogs` |
| `identifire` | String | Yes | Client identifier |
| `client_ver` | String | Yes | API version |
| `checksum` | String | Yes | Data integrity checksum |
| `ip_address` | String | Yes | Client IP address |
| `mac_address` | String | Yes | Client MAC address |
| `secret` | String | No | Secret key |
| `lastId` | String | No | Last log ID for pagination |
| `filters-url` | String | No | URL pattern filter (supports wildcards) |
| `output` | String | No | Output format (e.g., `json`) |
| `limit` | Integer | No | Number of logs to retrieve |
| `activeModule` | String | No | Filter by active module |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com" \
  -H "Content-Type: application/json" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "x-tts-module: SPIDER" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  -d '{
    "action": "liveLogs",
    "identifire": "www.example.com",
    "client_ver": "V4.21.04",
    "checksum": "generated_checksum_hash",
    "ip_address": "192.168.1.100",
    "mac_address": "-",
    "secret": "YOUR_SECRET_KEY",
    "lastId": "",
    "output": "json",
    "limit": 50
  }'
```

**Success Response:**

Returns live logs in the specified format.

---

### 11. Hit Log

Retrieve logs for a specific date.

**Endpoint:** `POST /{identifire}`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `action` | String | Yes | Set to `hitLog` |
| `identifire` | String | Yes | Client identifier |
| `client_ver` | String | Yes | API version |
| `checksum` | String | Yes | Data integrity checksum |
| `ip_address` | String | Yes | Client IP address |
| `mac_address` | String | Yes | Client MAC address |
| `logDate` | String | Yes | Date in YYYY-MM-DD format |
| `output` | String | No | Output format (e.g., `json`) |
| `secret` | String | No | Secret key |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com" \
  -H "Content-Type: application/json" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "x-tts-module: SPIDER" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  -d '{
    "action": "hitLog",
    "identifire": "www.example.com",
    "client_ver": "V4.21.04",
    "checksum": "generated_checksum_hash",
    "ip_address": "192.168.1.100",
    "mac_address": "-",
    "logDate": "2025-01-15",
    "output": "json",
    "secret": "YOUR_SECRET_KEY"
  }'
```

**Success Response:**

Returns hit logs for the specified date.

---

## Statistics

### 12. Fetch Statistics

Retrieve usage statistics by month or specific date.

**Endpoint:** `POST /{identifire}`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email
- `x-tts-module`: Set to `SPIDER`

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `action` | String | Yes | Set to `fetchStats` |
| `identifire` | String | Yes | Client identifier |
| `client_ver` | String | Yes | API version |
| `checksum` | String | Yes | Data integrity checksum |
| `ip_address` | String | Yes | Client IP address |
| `mac_address` | String | Yes | Client MAC address |
| `secret` | String | No | Secret key |
| `month` | String | No* | Month in YYYY-MM format |
| `date` | String | No* | Date in YYYY-MM-DD format |
| `output` | String | No | Output format (e.g., `json`) |

\* Provide either `month` or `date`, not both

**Example Request (Monthly Stats):**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com" \
  -H "Content-Type: application/json" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "x-tts-module: SPIDER" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  -d '{
    "action": "fetchStats",
    "identifire": "www.example.com",
    "client_ver": "V4.21.04",
    "checksum": "generated_checksum_hash",
    "ip_address": "192.168.1.100",
    "mac_address": "-",
    "month": "2025-01",
    "output": "json",
    "secret": "YOUR_SECRET_KEY"
  }'
```

**Example Request (Daily Stats):**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com" \
  -H "Content-Type: application/json" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "x-tts-module: SPIDER" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  -d '{
    "action": "fetchStats",
    "identifire": "www.example.com",
    "client_ver": "V4.21.04",
    "checksum": "generated_checksum_hash",
    "ip_address": "192.168.1.100",
    "mac_address": "-",
    "date": "2025-01-15",
    "output": "json",
    "secret": "YOUR_SECRET_KEY"
  }'
```

**Success Response:**

Returns statistics in the specified format.

---

## Spider Module

### 13. Delete Cache

Remove cached content for a specific URL.

**Endpoint:** `POST /https://{full_url_to_clear}`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email
- `x-tts-module`: Set to `SPIDER`

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `action` | String | Yes | Set to `deleteCache` |
| `identifire` | String | Yes | Client identifier |
| `client_ver` | String | Yes | API version |
| `secret` | String | No | Secret key |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/https://www.example.com/products/item-123" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-module: SPIDER" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  --data-urlencode "action=deleteCache" \
  --data-urlencode "identifire=www.example.com" \
  --data-urlencode "client_ver=V4.21.04"
```

**Success Response:**

Confirms cache deletion.

---

### 14. Purge Cache

Clear all cached content for the domain.

**Endpoint:** `POST /https://{domain}`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email
- `x-tts-module`: Set to `SPIDER`

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `action` | String | Yes | Set to `purge` |
| `identifire` | String | Yes | Client identifier |
| `client_ver` | String | Yes | API version |
| `secret` | String | No | Secret key |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/https://www.example.com" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-module: SPIDER" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  --data-urlencode "action=purge" \
  --data-urlencode "identifire=www.example.com" \
  --data-urlencode "client_ver=V4.21.04"
```

**Success Response:**

Confirms cache purge.

---

### 15. Fetch Customer Orders

Retrieve customer order information.

**Endpoint:** `POST /https://{domain}`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email
- `x-tts-module`: Set to `SPIDER`

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `action` | String | Yes | Set to `fetchCustomerOrders` |
| `identifire` | String | Yes | Client identifier |
| `client_ver` | String | Yes | API version |
| `secret` | String | No | Secret key |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/https://www.example.com" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-module: SPIDER" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  --data-urlencode "action=fetchCustomerOrders" \
  --data-urlencode "identifire=www.example.com" \
  --data-urlencode "client_ver=V4.21.04"
```

**Success Response:**

Returns customer orders in JSON format.

---

### 16. Get Warmup Cache Status

Check the status of cache warmup operations.

**Endpoint:** `POST /https://{domain}`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email
- `x-tts-module`: Set to `SPIDER`

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `action` | String | Yes | Set to `getWarmupCacheStatus` |
| `identifire` | String | Yes | Client identifier |
| `client_ver` | String | Yes | API version |
| `secret` | String | No | Secret key |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/https://www.example.com" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-module: SPIDER" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  --data-urlencode "action=getWarmupCacheStatus" \
  --data-urlencode "identifire=www.example.com" \
  --data-urlencode "client_ver=V4.21.04"
```

**Success Response:**

Returns cache warmup status information.

---

### 17. Spider Health Check

Check the health and availability of a cached URL.

**Endpoint:** `HEAD /https://{full_url}`

**Base URL:** `https://spider.services.utility-server.com`

**Required Headers:**
- `x-tts-cc-access-key`: Your access key
- `x-no-cache`: Set to `TRUE` to bypass cache

**Example Request:**

```bash
curl -I "https://spider.services.utility-server.com/https://www.example.com/products" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "x-no-cache: TRUE"
```

**Success Response:**

Returns HTTP headers with status information.

---

## Sitemap Management

### 18. Add Sitemap

Register a new sitemap XML file.

**Endpoint:** `POST /{identifire}`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `action` | String | Yes | Set to `addSitemap` |
| `identifire` | String | Yes | Client identifier |
| `client_ver` | String | Yes | API version |
| `sitemap` | String | Yes | Full URL to sitemap XML file |
| `secret` | String | No | Secret key |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  --data-urlencode "action=addSitemap" \
  --data-urlencode "identifire=www.example.com" \
  --data-urlencode "client_ver=V4.21.04" \
  --data-urlencode "sitemap=https://www.example.com/sitemap.xml"
```

**Success Response:**

Confirms sitemap has been added.

---

### 19. Update Sitemap

Update or replace an existing sitemap.

**Endpoint:** `POST /{identifire}`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `action` | String | Yes | Set to `updateSitemap` |
| `identifire` | String | Yes | Client identifier |
| `client_ver` | String | Yes | API version |
| `sitemap` | String | Yes | Full URL to new sitemap XML file |
| `secret` | String | No | Secret key |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  --data-urlencode "action=updateSitemap" \
  --data-urlencode "identifire=www.example.com" \
  --data-urlencode "client_ver=V4.21.04" \
  --data-urlencode "sitemap=https://www.example.com/sitemap-updated.xml"
```

**Success Response:**

Confirms sitemap has been updated.

---

### 20. Update TTL (Time To Live)

Set cache expiration time.

**Endpoint:** `POST /{identifire}`

**Required Headers:**
- `session-id`: Current session ID
- `member-email`: Logged-in user's email

**Request Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `action` | String | Yes | Set to `updateTTL` |
| `identifire` | String | Yes | Client identifier |
| `client_ver` | String | Yes | API version |
| `ttl` | Integer | Yes | Time to live in minutes |
| `secret` | String | No | Secret key |

**Example Request:**

```bash
curl -X POST "https://prod.services.utility-server.com/www.example.com" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  --data-urlencode "action=updateTTL" \
  --data-urlencode "identifire=www.example.com" \
  --data-urlencode "client_ver=V4.21.04" \
  --data-urlencode "ttl=60"
```

**Success Response:**

Confirms TTL has been updated.

---

## Request/Response Formats

### Content Types

The API supports two content types:

1. **application/json**
   - Preferred for most endpoints
   - Requires `checksum` for data integrity
   - Better for complex data structures

2. **application/x-www-form-urlencoded**
   - Supported for backward compatibility
   - Simpler for basic requests
   - No checksum required

### Checksum Generation

For JSON requests, generate a SHA-256 checksum of the request body:

```python
import hashlib
import json

def generate_checksum(data):
    json_string = json.dumps(data, sort_keys=True)
    return hashlib.sha256(json_string.encode()).hexdigest()
```

### User-Agent Format

The User-Agent should follow this pattern:
```
TTS-Utility-Client/{CLIENT_ID}.{ENVIRONMENT}.{CLIENT_TYPE}/{VERSION}
```

Examples:
- `TTS-Utility-Client/505.EZM.PROD/V4.21.04`
- `TTS-Utility-Client/504.EJM.PPRO/V4.21.04`
- `TTS-Utility-Client/69.ANS.OPEN/V4.21.02`

---

## Error Codes

### HTTP Status Codes

| Code | Description |
|------|-------------|
| 200 | Success |
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid credentials or session |
| 403 | Forbidden - Insufficient permissions |
| 404 | Not Found - Resource doesn't exist |
| 429 | Too Many Requests - Rate limit exceeded |
| 500 | Internal Server Error |
| 503 | Service Unavailable |

### Common Error Messages

| Error Message | Cause | Solution |
|---------------|-------|----------|
| `Active login session found, please logout existing session` | User already has an active session | Use terminate_session or member_logout endpoint |
| `Invalid credentials` | Incorrect email or password | Verify credentials and try again |
| `Session expired` | Session timeout reached | Login again to create a new session |
| `Invalid checksum` | Checksum verification failed | Regenerate checksum and retry |
| `Access key not found` | Invalid or expired access key | Verify access key or rotate to get a new one |
| `Rate limit exceeded` | Too many requests | Wait before retrying (see Rate Limiting) |

---

## Rate Limiting

### Default Limits

- **Login Attempts**: 5 per IP address per 15 minutes
- **API Calls**: 1000 requests per hour per access key
- **Log Queries**: 100 requests per hour per session

### Rate Limit Headers

All responses include rate limit information:

```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 847
X-RateLimit-Reset: 1640000000
```

### Handling Rate Limits

When rate limit is exceeded (HTTP 429), wait for the time specified in the `Retry-After` header before retrying.

---

## Best Practices

### Security

1. **Never expose credentials**: Keep access keys and secrets secure
2. **Use HTTPS**: Always use secure connections in production
3. **Rotate keys regularly**: Use the rotate endpoints periodically
4. **Validate checksums**: Always generate and verify checksums for JSON requests
5. **Implement proper session management**: Logout when sessions are no longer needed

### Performance

1. **Reuse sessions**: Don't create new sessions for every request
2. **Cache responses**: Cache statistical data and logs when appropriate
3. **Use pagination**: Utilize `lastId` parameter for log queries
4. **Implement exponential backoff**: For retry logic on failures
5. **Monitor rate limits**: Track your usage to avoid throttling

### Integration

1. **Version your requests**: Always include the correct `client_ver`
2. **Log requests and responses**: Maintain audit trails for debugging
3. **Handle errors gracefully**: Implement proper error handling for all endpoints
4. **Use appropriate content types**: Choose JSON for complex requests
5. **Set proper timeouts**: Don't let requests hang indefinitely

### Data Integrity

1. **Always include required headers**: Missing headers will cause failures
2. **Validate input data**: Check parameters before sending requests
3. **Use correct date formats**: Follow YYYY-MM-DD and YYYY-MM formats
4. **Handle pagination correctly**: Track `lastId` for continued queries
5. **Verify responses**: Check response status and content before proceeding

---

## Support and Resources

### Documentation

- **API Version**: 4.21.04
- **Last Updated**: November 6, 2025
- **Status Page**: https://status.utility-server.com

### Contact

For technical support, integration assistance, or to report issues:

- **Email**: support@utility-server.com
- **Technical Documentation**: https://docs.utility-server.com
- **Developer Portal**: https://developers.utility-server.com

### Changelog

#### Version 4.21.04
- Added support for activeModule parameter in liveLogs
- Improved checksum validation
- Enhanced error messages

#### Version 4.21.03
- Added date-specific statistics fetching
- Improved rate limiting
- Bug fixes for session management

#### Version 4.21.02
- Added key rotation endpoints
- Enhanced security features
- Performance improvements

---

## Appendix

### Example: Complete Authentication Flow

```bash
# Step 1: Login
curl -X POST "https://prod.services.utility-server.com/www.example.com/customers/member_login" \
  -H "Content-Type: application/json" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -d '{
    "email_id": "user@example.com",
    "password": "secure_password",
    "identifire": "www.example.com",
    "client_ver": "V4.21.04",
    "checksum": "generated_checksum",
    "ip_address": "192.168.1.100",
    "mac_address": "-",
    "secret": "YOUR_SECRET_KEY"
  }'

# Response: {"session_id":"abc123","user_type":"client","message":"Logged-in successful..."}

# Step 2: Use authenticated endpoint
curl -X POST "https://prod.services.utility-server.com/www.example.com/customers/get_users" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "session-id: abc123" \
  -H "member-email: user@example.com" \
  --data-urlencode "identifire=www.example.com" \
  --data-urlencode "client_ver=V4.21.04"

# Step 3: Logout
curl -X POST "https://prod.services.utility-server.com/www.example.com/customers/member_logout" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "session-id: abc123" \
  -H "member-email: user@example.com" \
  --data-urlencode "identifire=www.example.com" \
  --data-urlencode "client_ver=V4.21.04"
```

### Example: Monitoring and Analytics Flow

```bash
# Fetch live logs
curl -X POST "https://prod.services.utility-server.com/www.example.com" \
  -H "Content-Type: application/json" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "x-tts-module: SPIDER" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  -d '{
    "action": "liveLogs",
    "identifire": "www.example.com",
    "client_ver": "V4.21.04",
    "checksum": "generated_checksum",
    "ip_address": "192.168.1.100",
    "mac_address": "-",
    "output": "json",
    "limit": 50,
    "secret": "YOUR_SECRET_KEY"
  }'

# Fetch monthly statistics
curl -X POST "https://prod.services.utility-server.com/www.example.com" \
  -H "Content-Type: application/json" \
  -H "User-Agent: TTS-Utility-Client/V4.21.04" \
  -H "x-tts-cc-access-key: YOUR_ACCESS_KEY" \
  -H "x-tts-module: SPIDER" \
  -H "session-id: YOUR_SESSION_ID" \
  -H "member-email: user@example.com" \
  -d '{
    "action": "fetchStats",
    "identifire": "www.example.com",
    "client_ver": "V4.21.04",
    "checksum": "generated_checksum",
    "ip_address": "192.168.1.100",
    "mac_address": "-",
    "month": "2025-01",
    "output": "json",
    "secret": "YOUR_SECRET_KEY"
  }'
```

---

**End of Documentation**

© 2024 TTS Utility Server. All rights reserved.
