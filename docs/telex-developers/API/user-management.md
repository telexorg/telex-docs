# User Management

## Overview
The **Telex User Management API** provides endpoints to manage users within Telex. This includes retrieving users of an organization, retrieving organization metrics, updating a user's role and status, removing a user from an organization, and retrieving invitations for an organization. These endpoints ensure efficient handling of user management operations and data integration.

### Retrieve Users of an Organization
- **Endpoint:** `GET /organisations/{org_id}/users`
- **Tags:** user-management
- **Summary:** Retrieve users of an organization
- **Security:** 
    - `bearerAuth: []`

#### Parameters
- **org_id**: The ID of the organization (path parameter, required, UUID).
- **page**: The page number for pagination (query parameter, optional, default: 1).
- **page_size**: The number of items per page for pagination (query parameter, optional, default: 20).

#### Responses
- **200**: Users retrieved successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Users retrieved successfully",
    "data": [
        {
            "id": "uuid",
            "email": "user@example.com",
            "phone_number": "123-456-7890",
            "name": "John Doe",
            "role": "admin",
            "status": "active"
        }
        // More user objects...
    ],
    "pagination": {
        "current_page": 1,
        "page_size": 20,
        "total_items": 100,
        "total_pages": 5
    }
}
```
- **400**: Bad request.
```json
{
    "status": "error",
    "status_code": 400,
    "message": "Bad request",
    "errors": ["Validation error details"]
}
```
- **401**: Unauthorized.
```json
{
    "status": "error",
    "status_code": 401,
    "message": "Unauthorized access"
}
```
- **403**: Forbidden.
```json
{
    "status": "error",
    "status_code": 403,
    "message": "Access forbidden"
}
```

---

### Retrieve Organization Metrics
- **Endpoint:** `GET /organisations/{org_id}/metrics`
- **Tags:** user-management
- **Summary:** Retrieve organization metrics
- **Security:** 
    - `bearerAuth: []`

#### Parameters
- **org_id**: The ID of the organization (path parameter, required, UUID).
- **days**: Number of days for metrics (query parameter, required, default: 7).

#### Responses
- **200**: Metrics fetched successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Metrics fetched successfully",
    "data": {
        "active_count": 50,
        "inactive_count": 10,
        "total_members": 60
    }
}
```
- **400**: Bad request.
```json
{
    "status": "error",
    "status_code": 400,
    "message": "Bad request",
    "errors": ["Validation error details"]
}
```
- **401**: Unauthorized.
```json
{
    "status": "error",
    "status_code": 401,
    "message": "Unauthorized access"
}
```
- **403**: Forbidden.
```json
{
    "status": "error",
    "status_code": 403,
    "message": "Access forbidden"
}
```

---

### Update a User's Role and Status in an Organization
- **Endpoint:** `PUT /organisations/{org_id}/users/{user_id}`
- **Tags:** user-management
- **Summary:** Update a user's role and status in an organization
- **Security:** 
    - `bearerAuth: []`

#### Parameters
- **org_id**: The ID of the organization (path parameter, required, UUID).
- **user_id**: The ID of the user (path parameter, required, UUID).

#### Request Body
```json
{
    "role_id": 2,
    "status": "deactivated"
}
```

#### Responses
- **200**: User role and status updated successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "success",
    "data": {
        "user_id": "uuid",
        "organisation_id": "uuid",
        "status": "deactivated",
        "role_id": 2,
        "created_at": "2024-01-01T00:00:00Z",
        "deleted_at": null
    }
}
```
- **400**: Bad request.
```json
{
    "status": "error",
    "status_code": 400,
    "message": "Bad request",
    "errors": ["Validation error details"]
}
```
- **401**: Unauthorized.
```json
{
    "status": "error",
    "status_code": 401,
    "message": "Unauthorized access"
}
```
- **403**: Forbidden.
```json
{
    "status": "error",
    "status_code": 403,
    "message": "Access forbidden"
}
```

---

### Remove a User from an Organization
- **Endpoint:** `DELETE /organisations/{org_id}/users/{user_id}`
- **Tags:** user-management
- **Summary:** Remove a user from an organization
- **Security:** 
    - `bearerAuth: []`

#### Parameters
- **org_id**: The ID of the organization (path parameter, required, UUID).
- **user_id**: The ID of the user to be removed (path parameter, required, UUID).

#### Responses
- **200**: User removed successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "success",
    "data": "Member removed successfully"
}
```
- **400**: Bad request.
```json
{
    "status": "error",
    "status_code": 400,
    "message": "Bad request",
    "errors": ["Validation error details"]
}
```
- **401**: Unauthorized.
```json
{
    "status": "error",
    "status_code": 401,
    "message": "Unauthorized access"
}
```
- **403**: Forbidden.
```json
{
    "status": "error",
    "status_code": 403,
    "message": "Access forbidden"
}
```

---

### Retrieve Invitations for an Organization
- **Endpoint:** `GET /organisations/{org_id}/invites`
- **Tags:** user-management
- **Summary:** Retrieve invitations for an organization
- **Security:** 
    - `bearerAuth: []`

#### Parameters
- **org_id**: The ID of the organization (path parameter, required, UUID).
- **page**: The page number for pagination (query parameter, optional, default: 1).
- **page_size**: The number of items per page for pagination (query parameter, optional, default: 20).

#### Responses
- **200**: Invitations retrieved successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "success",
    "data": {
        "invitations": [
            {
                "id": "uuid",
                "email": "invitee@example.com",
                "token": "invitation_token",
                "status": "invited",
                "role": "admin",
                "organisation_id": "uuid",
                "is_telex_user": false,
                "Organisation": {
                    "id": "uuid",
                    "name": "Org Name",
                    "description": "Organization Description",
                    "email": "org@example.com",
                    "type": "type",
                    "location": "Location",
                    "country": "Country",
                    "owner_id": "uuid",
                    "channels_count": 0,
                    "total_messages_count": 0,
                    "org_roles": ["role1", "role2"],
                    "Users": ["user1", "user2"],
                    "created_at": "2024-01-01T00:00:00Z",
                    "updated_at": "2024-01-01T00:00:00Z"
                },
                "created_at": "2024-01-01T00:00:00Z",
                "expires_at": "2024-01-08T00:00:00Z"
            }
            // More invitation objects...
        ],
        "totalnoguests": 5
    },
    "pagination": {
        "current_page": 1,
        "page_count": 2,
        "total_pages_count": 1
    }
}
```
- **400**: Bad request.
```json
{
    "status": "error",
    "status_code": 400,
    "message": "Bad request",
    "errors": ["Validation error details"]
}
```
- **401**: Unauthorized.
```json
{
    "status": "error",
    "status_code": 401,
    "message": "Unauthorized access"
}
```
- **403**: Forbidden.
```json
{
    "status": "error",
    "status_code": 403,
    "message": "Access forbidden"
}
```
