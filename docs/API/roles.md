# Organization Roles 

## Overview
This document provides an overview of the organization roles endpoints available in the Telex API. These endpoints allow for the management of roles within organizations.

---

## Organization Roles Endpoints

### 1. Create a New Organization Role
- **Endpoint:** `POST /organisations/{orgId}/roles`
- **Description:** This endpoint creates a new role within the specified organization.

#### Parameters
- **orgId** (path): ID of the organization to create the role in (UUID).

#### Request Body
```json
{
  "name": "string",        // Name of the role
  "description": "string"  // Description of the role
}
```

#### Responses
- **201**: Role created successfully.
  ```json
  {
    "status": "string",
    "status_code": 201,
    "message": "string"
  }
  ```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 2. Retrieve a List of Organization Roles
- **Endpoint:** `GET /organisations/{orgId}/roles`
- **Description:** This endpoint retrieves a list of roles within the specified organization.

#### Parameters
- **orgId** (path): ID of the organization to retrieve roles for (UUID).

#### Responses
- **200**: Roles retrieved successfully.
  ```json
  {
    "roles": [ /* Array of role objects */ ]
  }
  ```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 3. Retrieve a Specific Organization Role
- **Endpoint:** `GET /organisations/{orgId}/roles/{roleId}`
- **Description:** This endpoint retrieves detailed information about a specific role within the specified organization.

#### Parameters
- **orgId** (path): ID of the organization containing the role (UUID).
- **roleId** (path): ID of the role to retrieve (UUID).

#### Responses
- **200**: Role retrieved successfully.
  ```json
  {
    "id": "string",
    "name": "string",
    "description": "string",
    "organisation_id": "string"
  }
  ```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 4. Update a Specific Organization Role
- **Endpoint:** `PUT /organisations/{orgId}/roles/{roleId}`
- **Description:** This endpoint updates the details of a specific role within the specified organization.

#### Parameters
- **orgId** (path): ID of the organization containing the role (UUID).
- **roleId** (path): ID of the role to update (UUID).

#### Request Body
```json
{
  "name": "string",        // Updated name of the role
  "description": "string"  // Updated description of the role
}
```

#### Responses
- **200**: Role updated successfully.
  ```json
  {
    "status": "string",
    "status_code": 200,
    "message": "string"
  }
  ```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 5. Delete a Specific Organization Role
- **Endpoint:** `DELETE /organisations/{orgId}/roles/{roleId}`
- **Description:** This endpoint deletes a specific role within the specified organization.

#### Parameters
- **orgId** (path): ID of the organization containing the role (UUID).
- **roleId** (path): ID of the role to delete (UUID).

#### Responses
- **200**: Role deleted successfully.
  ```json
  {
    "status": "string",
    "status_code": 200,
    "message": "string"
  }
  ```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **404**: Not found.
- **500**: Server error.

---

### 6. Update Permissions for a Specific Role
- **Endpoint:** `PUT /organisations/{orgId}/roles/{roleId}/permissions`
- **Description:** This endpoint updates the permissions for a specific role within the specified organization.

#### Parameters
- **orgId** (path): ID of the organization containing the role (UUID).
- **roleId** (path): ID of the role to update permissions for (UUID).

#### Request Body
```json
{
  "permission_list": {
    "permission_name": true | false
  }
}
```

#### Responses
- **200**: Permissions updated successfully.
  ```json
  {
    "status": "string",
    "status_code": 200,
    "message": "string"
  }
  ```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 7. Assign User to Another Role
- **Endpoint:** `PUT /users/switch-roles`
- **Description:** This endpoint assigns a user to a different role within an organization.

#### Request Body
```json
{
  "user_id": "string",  // User ID (UUID)
  "org_id": "string",   // Organization ID (UUID)
  "role_id": "string"   // Role ID (UUID)
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
