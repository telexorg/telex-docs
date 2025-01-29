# Authentication

## Overview
The **Telex Authentication API** provides a suite of endpoints to manage user authentication securely and effectively. This includes user registration, login, logout, password management, magic link authentication, and email verification. These endpoints are designed to integrate seamlessly into your application, ensuring a smooth user experience while maintaining robust security practices.

The API supports features like OAuth integration, onboarding status tracking, and token-based verification, enabling developers to implement advanced authentication flows with minimal effort.

---

## Authentication Endpoints

## Base URL - 

### production - `https://api.telex.im/api/v1/` 

### staging - `https://api.staging.telex.im/api/v1/`

### 1. **Register a New User**
- **Endpoint:** `POST /auth/register`
- **Purpose:** Creates a new user account in the Telex platform. This step is required before accessing other services.
<!-- - **Use Case:** Initial user signup process. -->

#### Request Body
```json
{
  "username": "string",
  "email": "string",
  "password": "string",
  "first_name": "string",
  "last_name": "string"
}
```
- **Required Fields:** `email`, `password`
- **Optional Fields:** `username`, `first_name`, `last_name`

#### Responses
- **201 Created:** User successfully registered.
  ```json
  {
    "status": "success",
    "message": "User created successfully",
    "status_code": 201
  }
  ```
- **400 Bad Request:** Invalid input (e.g., missing required fields or improperly formatted data).
- **422 Unprocessable Entity:** Email or username already exists.

---

### 2. **Authenticate a User**
- **Endpoint:** `POST /auth/login`
- **Purpose:** Verifies user credentials and issues an access token for subsequent API calls.
<!-- - **Use Case:** Login for returning users. -->

#### Request Body
```json
{
  "email": "string",
  "password": "string"
}
```
- **Required Fields:** `email`, `password`

#### Responses
- **200 OK:** Login successful; returns an access token.
  ```json
  {
    "status": "success",
    "message": "Login successful",
    "status_code": 200,
    "data": {
      "user": { /* User details */ },
      "access_token": "access_token"
    }
  }
  ```
- **401 Unauthorized:** Incorrect email or password.
- **422 Unprocessable Entity:** Validation error (e.g., missing fields).

---

### 3. **Logout a User**
- **Endpoint:** `POST /auth/logout`
- **Purpose:** Ends the current user session by invalidating the token.
<!-- - **Use Case:** When a user manually logs out. -->

#### Responses
- **200 OK:** Logout successful.
  ```json
  {
    "status": "success",
    "message": "Logout successful",
    "status_code": 200
  }
  ```
- **401 Unauthorized:** Token missing or invalid.
- **400 Bad Request:** Invalid input provided.

---

### 4. **Google OAuth Callback**
- **Endpoint:** `POST /auth/google`
- **Purpose:** Processes OAuth tokens from Google to authenticate users.
<!-- - **Use Case:** Allows users to log in or register using their Google account. -->

#### Request Body
```json
{
  "id_token": "string"
}
```
- **Required Fields:** `id_token`

#### Responses
- **200 OK:** OAuth login successful; returns user details and access token.
- **400 Bad Request:** Invalid or missing `id_token`.
- **401 Unauthorized:** Token verification failed.

---

### 5. **Change Password**
- **Endpoint:** `PUT /auth/change-password`
- **Purpose:** Allows users to update their password after verifying the old one.
<!-- - **Use Case:** Enhances account security by enabling password updates. -->

#### Request Body
```json
{
  "old_password": "string",
  "new_password": "string"
}
```
- **Required Fields:** `old_password`, `new_password`

#### Responses
- **200 OK:** Password updated successfully.
- **400 Bad Request:** Missing or invalid fields.
- **401 Unauthorized:** Old password incorrect.
- **422 Unprocessable Entity:** New password does not meet criteria.

---

### 6. **Request Magic Link**
- **Endpoint:** `POST /auth/magic-link`
- **Purpose:** Sends a magic link to the user's email for passwordless login.
<!-- - **Use Case:** Facilitates frictionless authentication without remembering passwords. -->

#### Request Body
```json
{
  "email": "string"
}
```
- **Required Fields:** `email`

#### Responses
- **200 OK:** Magic link sent successfully.
- **400 Bad Request:** Invalid or missing email address.
- **500 Internal Server Error:** Failed to process the request.

---

### 7. **Verify Magic Link**
- **Endpoint:** `POST /auth/magic-link/verify`
- **Purpose:** Validates the magic link token to authenticate the user.
<!-- - **Use Case:** Completes the passwordless login flow. -->

#### Request Body
```json
{
  "token": "string"
}
```
- **Required Fields:** `token`

#### Responses
- **200 OK:** Token verified successfully; returns user details.
- **400 Bad Request:** Invalid token.
- **422 Unprocessable Entity:** Token expired or already used.

---

### 8. **Request Password Reset**
- **Endpoint:** `POST /auth/password-reset`
- **Purpose:** Sends a password reset link to the user’s email.
<!-- - **Use Case:** Helps users regain access to their account. -->

#### Request Body
```json
{
  "email": "string"
}
```
- **Required Fields:** `email`

#### Responses
- **200 OK:** Password reset link sent.
- **400 Bad Request:** Invalid or missing email address.
- **500 Internal Server Error:** Failed to process the request.

---

### 9. **Verify Password Reset Token**
- **Endpoint:** `POST /auth/password-reset/verify`
- **Purpose:** Validates the token and updates the password.
<!-- - **Use Case:** Completes the password reset process. -->

#### Request Body
```json
{
  "token": "string",
  "new_password": "string"
}
```
- **Required Fields:** `token`, `new_password`

#### Responses
- **200 OK:** Password reset successful.
- **400 Bad Request:** Invalid or missing fields.
- **422 Unprocessable Entity:** Token expired or invalid.

---

### 10. **Request Email Verification**
- **Endpoint:** `POST /auth/email-request`
- **Purpose:** Sends a verification email to the user.
<!-- - **Use Case:** Ensures user-provided email addresses are valid. -->

#### Request Body
```json
{
  "email": "string"
}
```
- **Required Fields:** `email`

#### Responses
- **200 OK:** Verification email sent.
- **400 Bad Request:** Invalid or missing email.
- **422 Unprocessable Entity:** Email already verified.

---

### 11. **Verify Email Token**
- **Endpoint:** `POST /auth/email-request/verify`
- **Purpose:** Confirms the user’s email address using the provided token.
<!-- - **Use Case:** Completes the email verification process. -->

#### Request Body
```json
{
  "token": "string"
}
```
- **Required Fields:** `token`

#### Responses
- **200 OK:** Email verified successfully.
- **400 Bad Request:** Invalid token.
- **422 Unprocessable Entity:** Token expired or invalid.

---

