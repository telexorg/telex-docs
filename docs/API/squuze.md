# Squeeze

## Overview
The **Telex Squeeze API** provides endpoints to manage opt-in notifications within Telex. This includes adding new entries to the opt-in list for receiving updates and notifications. These endpoints ensure efficient handling of user opt-ins and data integration.

### 1. Opt-in for Notifications
- **Endpoint:** `/optin`
- **Method:** `POST`
- **Tags:** Squeeze
- **Summary:** Add a new entry to the opt-in list for receiving updates and notifications.

#### Request Body
```json
{
  "first_name": "John",
  "last_name": "Doe",
  "email": "johndoe@example.com"
}
```

#### Responses
- **201**: Opt-in record created successfully.
```json
{
  "status": "success",
  "status_code": 201,
  "message": "Opt-in record created successfully"
}
```
- **400**: Bad Request - Invalid data.
- **401**: Authentication error.
- **409**: Conflict - Email already opted in.
- **500**: Server error.