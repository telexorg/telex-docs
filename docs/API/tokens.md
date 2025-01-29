# Token

## Overview
The **Telex Token API** provides endpoints to generate tokens for various purposes within your application. This includes generating connection tokens for establishing server connections to Centrifugo and subscription tokens for subscribing to specific channels.


### 1. Generate Connection Token
- **Endpoint:** `GET /token/connection`
- **Summary:** Generate connection token
- **Security:** bearerAuth
- **Description:** Retrieve a connection token for establishing a connection to the server.

#### Responses
- **200**: Connection token retrieved successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Connection token retrieved successfully",
  "data": {
    "token": "your_connection_token_here",
  }
}
```
- **400**: Bad request.
```json
{
  "status": "error",
  "status_code": 400,
  "message": "Bad request",
  "error": "Invalid parameters"
}
```
- **401**: Unauthorized.
```json
{
  "status": "error",
  "status_code": 401,
  "message": "Unauthorized",
  "error": "Invalid or missing authentication token"
}
```

---

### 2. Generate Subscription Token
- **Endpoint:** `POST /token/subscription`
- **Summary:** Generate subscription token
- **Security:** bearerAuth
- **Description:** Generate a token for subscribing to a specific channel.

#### Request Body
- **Content-Type:** `application/json`
- **Required:** true
```json
{
  "channel_id": "01910544-d1e1-7ada-bdac-c761e527ec91"
}
```

#### Responses
- **200**: Subscription token generated successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Subscription token generated successfully",
  "data": {
    "token": "your_subscription_token_here",
  }
}
```
- **400**: Bad request.
```json
{
  "status": "error",
  "status_code": 400,
  "message": "Bad request",
  "error": "Invalid parameters"
}
```
- **401**: Unauthorized.
```json
{
  "status": "error",
  "status_code": 401,
  "message": "Unauthorized",
  "error": "Invalid or missing authentication token"
}
```

---
