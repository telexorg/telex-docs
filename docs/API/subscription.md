# Subscription

## Overview
The **Telex Subscription API** provides endpoints to manage user subscriptions, including creating, modifying, listing, completing, and deleting subscriptions. It also allows retrieval of associated invoices.

### Complete a Subscription
- **Endpoint:** `/subscriptions/complete`
- **Method:** `POST`
- **Summary:** Complete a subscription and retrieve associated invoices
- **Security:** 
    - `bearerAuth: []`

#### Request Body
```json
{
    "org_id": "01915c5a-cc3b-7357-998e-315fcb18fceb",
    "stripe_session_id": "cs_test_a1WVERDendXHiw3J0qA4y40V0TGd0AM2lRTGlukEEp12znwwFyuVg0eO5Z"
}
```

#### Responses
- **200**: Subscription completed successfully and invoices retrieved.
```json
{
    "invoice_items": [
        {
            "id": "invoice_1",
            "amount_due": 1000,
            "status": "paid",
            "created": 1620000000
        }
        // More invoice objects...
    ]
}
```
- **400**: Bad request, either the session was not paid or no subscription ID found.
```json
{
    "error": "Error message explaining the failure"
}
```
- **500**: Internal server error.
```json
{
    "error": "Error message explaining the server failure"
}
```

---

### Create a New Subscription
- **Endpoint:** `/subscriptions/create`
- **Method:** `POST`
- **Summary:** Create a new subscription
- **Security:** 
    - `bearerAuth: []`

#### Request Body
```json
{
    "plan_name": "Premium",
    "org_id": "123456",
    "email": "user@example.com"
}
```

#### Responses
- **201**: Subscription created successfully.
```json
{
    "subscription_id": "sub_12345",
    "status": "active"
}
```
- **400**: Bad request.
```json
{
    "status": "error",
    "message": "Bad request",
    "errors": ["Validation error details"]
}
```
- **401**: Unauthorized.
- **500**: Internal server error.
```json
{
    "status": "error",
    "message": "Internal server error"
}
```

---

### List Subscriptions for a User
- **Endpoint:** `/subscriptions/list/{orgId}`
- **Method:** `GET`
- **Summary:** List subscriptions for a user
- **Security:** 
    - `bearerAuth: []`

#### Parameters
- **orgId** (path): The unique identifier for the organization.

#### Responses
- **200**: Subscriptions retrieved successfully.
```json
{
    "subscriptions": [
        {
            "id": "sub_12345",
            "plan_name": "Premium",
            "status": "active"
        }
        // More subscription objects...
    ]
}
```
- **401**: Unauthorized.
- **404**: User not found.
```json
{
    "status": "error",
    "message": "User not found"
}
```
- **500**: Internal server error.
```json
{
    "status": "error",
    "message": "Internal server error"
}
```

---

### Modify an Existing Subscription
- **Endpoint:** `/subscriptions/modify`
- **Method:** `PUT`
- **Summary:** Modify an existing subscription
- **Security:** 
    - `bearerAuth: []`

#### Request Body
```json
{
    "org_id": "01915c5a-cc3b-7357-998e-315fcb18fceb",
    "plan_name": "Basic"
}
```

#### Responses
- **200**: Subscription modified successfully.
```json
{
    "status": "success",
    "message": "Subscription modified successfully"
}
```
- **400**: Bad request.
```json
{
    "status": "error",
    "message": "Bad request",
    "errors": ["Validation error details"]
}
```
- **401**: Unauthorized.
- **500**: Internal server error.
```json
{
    "status": "error",
    "message": "Internal server error"
}
```

---

### Download Invoice Items
- **Endpoint:** `/invoice/download/sessionId/orgId`
- **Method:** `GET`
- **Summary:** Complete a subscription and retrieve invoice items
- **Parameters:**
    - **sessionId** (query): The ID of the Stripe Checkout Session.
    - **orgId** (query): The ID of the organization whose subscription is being completed.

#### Responses
- **200**: Successful operation.
```json
{
    "invoice_items": [
        {
            "id": "invoice_1",
            "amount_due": 1000,
            "status": "paid",
            "created": 1620000000
        }
        // More invoice objects...
    ]
}
```
- **400**: Bad request.
```json
{
    "error": "Error message explaining the failure"
}
```
- **500**: Internal server error.
```json
{
    "error": "Error message explaining the server failure"
}
```

---

### Delete a Subscription
- **Endpoint:** `/subscriptions/{orgId}`
- **Method:** `DELETE`
- **Summary:** Delete a subscription
- **Security:** 
    - `bearerAuth: []`

#### Parameters
- **orgId** (path): The ID of the user whose subscription is to be deleted.

#### Responses
- **200**: Subscription deleted successfully.
```json
{
    "status": "success",
    "message": "Subscription deleted successfully"
}
```
- **400**: Bad request.
```json
{
    "status": "error",
    "message": "Bad request",
    "errors": ["Validation error details"]
}
```
- **401**: Unauthorized.
- **500**: Internal server error.
```json
{
    "status": "error",
    "message": "Internal server error"
}
```
