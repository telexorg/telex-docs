# Telex User API Documentation

## Overview
This document provides an overview of the user-related endpoints available in the Telex API, including organization details, role assignments, login audits, session management, and organization switching.

---

## User Endpoints

### 1. Retrieve Organisation Details by Organisation ID
- **Endpoint:** `GET /users/organisations/{org_id}`
- **Summary:** Retrieve organisation details by organisation ID.
- **Security:** bearerAuth

#### Parameters
- **org_id** (path): The ID of the organisation whose details are being retrieved (required).

#### Responses
- **200**: Organisation details retrieved successfully.
    ```json
    {
        "status": "success",
        "message": "Organisation details retrieved successfully",
        "data": {
            "id": "string",
            "name": "string",
            "description": "string",
            "email": "string",
            "type": "string",
            "location": "string",
            "country": "string",
            "owner_id": "string",
            "channels_count": 0,
            "total_messages_count": 0,
            "org_roles": [],
            "users": [],
            "channels": [],
            "created_at": "string",
            "updated_at": "string"
        }
    }
    ```
- **400**: Bad request.
- **404**: Organisation not found.
- **500**: Internal server error.

---

### 2. Assign User to Another Role
- **Endpoint:** `PUT /users/switch-roles`
- **Summary:** Assign user to another role.
- **Security:** bearerAuth

#### Request Body
```json
{
    "user_id": "string",
    "org_id": "string",
    "role_id": "string"
}
```
**Required Fields:** `user_id`, `org_id`, `role_id`

#### Responses
- **200**: Role updated successfully.
    ```json
    {
        "status": "success",
        "message": "Role updated successfully",
        "status_code": 200
    }
    ```
- **400**: Bad request.
- **404**: Not found.
- **401**: Unauthorized.

---

### 3. Fetch User Role in an Organisation
- **Endpoint:** `GET /users/{user_id}/organisations/{organisation_id}/roles`
- **Summary:** Fetch user role in an organisation.

#### Parameters
- **user_id** (path): ID of the user (required).
- **organisation_id** (path): ID of the organisation (required).

#### Responses
- **200**: User role fetched successfully.
    ```json
    {
        "status": "success",
        "status_code": 200,
        "message": "User role fetched successfully",
        "data": {
            "role_id": "string",
            "role_name": "string",
            "organisation_id": "string",
            "organisation_name": "string"
        }
    }
    ```
- **404**: User or organisation not found.

---

### 4. Retrieve Login Audit Logs
- **Endpoint:** `GET /users/{userId}/login-audit`
- **Summary:** Retrieve login audit logs.
- **Security:** bearerAuth

#### Parameters
- **userId** (path): ID of the user (required).

#### Responses
- **200**: Login audit logs retrieved successfully.
- **400**: Bad request.
- **401**: Unauthorized.
- **500**: Server error.

---

### 5. Revoke a User Session
- **Endpoint:** `PUT /users/revoke-session`
- **Summary:** Revoke a user session.
- **Security:** bearerAuth

#### Request Body
```json
{
    "session_id": "string" // The ID of the session to revoke
}
```

#### Responses
- **200**: Session revoked successfully.
    ```json
    {
        "status": "success",
        "status_code": 200,
        "message": "Session revoked successfully"
    }
    ```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 6. Switch to an Organisation
- **Endpoint:** `PUT /users/switch-org`
- **Summary:** Switch to an organisation.
- **Security:** bearerAuth

#### Request Body
```json
{
    "current_org": "string" // The ID of the current organization
}
```
**Required Fields:** `current_org`

#### Responses
- **200**: Organizations switched successfully.
    ```json
    {
        "status": "success",
        "message": "Organization switched successfully"
    }
    ```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 7. Retrieve a User's Organizations
- **Endpoint:** `GET /users/organisations`
- **Summary:** Retrieve a list of organizations associated with a user.
- **Security:** bearerAuth

#### Responses
- **200**: Organizations retrieved successfully.
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 8. Retrieve a Specific User
- **Endpoint:** `GET /users/{userId}`
- **Summary:** Retrieve details of a specific user by their ID.
- **Security:** bearerAuth

#### Parameters
- **userId** (path): ID of the user to retrieve (required).

#### Responses
- **200**: User retrieved successfully.
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 9. Delete a Specific User
- **Endpoint:** `DELETE /users/{userId}`
- **Summary:** Delete a specific user by their ID.
- **Security:** bearerAuth

#### Parameters
- **userId** (path): ID of the user to delete (required).

#### Responses
- **200**: User deleted successfully.
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **404**: Not found.
- **500**: Server error.

---

### 10. Update a Specific User
- **Endpoint:** `PUT /users/{userId}`
- **Summary:** Update the details of a specific user by their ID.
- **Security:** bearerAuth

#### Parameters
- **userId** (path): ID of the user to update (required).

#### Request Body
- **Required Fields:** User details to update.

#### Responses
- **200**: User updated successfully.
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 11. Deactivate a Specific User
- **Endpoint:** `DELETE /users/deactivate/{userId}`
- **Summary:** Deactivate a specific user by their ID.
- **Security:** bearerAuth

#### Parameters
- **userId** (path): ID of the user to deactivate (required).

#### Responses
- **200**: User deactivated successfully.
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **404**: Not found.
- **500**: Server error.

---

### 12. Reactivate a Specific User
- **Endpoint:** `PUT /users/reactivate/{userId}`
- **Summary:** Reactivate a specific user by their ID.
- **Security:** bearerAuth

#### Parameters
- **userId** (path): ID of the user to activate (required).

#### Responses
- **200**: User activated successfully.
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **404**: Not found.
- **500**: Server error.

---

### 13. Retrieve a List of Users
- **Endpoint:** `GET /users`
- **Summary:** Retrieve a list of all users in the system.
- **Security:** bearerAuth

#### Responses
- **200**: Users retrieved successfully.
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 14. Get User Notification Preferences
- **Endpoint:** `GET /users/notification-preferences`
- **Summary:** Retrieves the notification preferences for a specific user.
- **Security:** bearerAuth

#### Responses
- **200**: User notification preferences retrieved successfully.
    ```json
    {
        "status": "success",
        "message": "User notification preferences retrieved successfully",
        "data": {
            "notify_about": {
                "option": "all_new_messages" // or "direct_messages_mentions" or "nothing"
            },
            "notification_schedule": true,
            "from_hour": "09:00",
            "to_hour": "17:00",
            "notification_method_email": true
        }
    }
    ```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 15. Update User Notification Preferences
- **Endpoint:** `PUT /users/notification-preferences`
- **Summary:** Updates the notification preferences for a specific user.
- **Security:** bearerAuth

#### Request Body
```json
{
    "notify_about": {
        "option": "all_new_messages" // or "direct_messages_mentions" or "nothing"
    },
    "notification_schedule": true,
    "from_hour": "09:00",
    "to_hour": "17:00",
    "notification_method_email": true
}
```
**Required Fields:** `notify_about`, `notification_schedule`, `from_hour`, `to_hour`, `notification_method_email`

#### Responses
- **200**: User notification preferences updated successfully.
    ```json
    {
        "status": "success",
        "message": "User notification preferences updated successfully",
        "data": {
            "notify_about": {
                "option": "all_new_messages" // or "direct_messages_mentions" or "nothing"
            },
            "notification_schedule": true,
            "from_hour": "09:00",
            "to_hour": "17:00",
            "notification_method_email": true
        }
    }
    ```
- **400**: Bad request.
- **401**: Unauthorized.
- **422**: Validation error.
- **500**: Server error.

