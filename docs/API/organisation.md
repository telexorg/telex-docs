
### 1. Create an Organization
- **Endpoint:** `POST /organisations`
- **Summary:** Create an organization
- **Security:** bearerAuth
- **Description:** Creates an organization.

#### Request Body
- **Content-Type:** `application/json`
- **Required:** true
```json
{
  "name": "string",
  "description": "string",
  "email": "string",
  "type": "string",
  "location": "string",
  "country": "string",
  "logo_url": "string"
}
```

#### Responses
- **201**: Organization created successfully.
  ```json
  {
    "status": "success",
    "status_code": 201,
    "message": "Organization created successfully",
    "data": {
      "id": "01910544-d1e1-7ada-bdac-c761e527ec91",
      "name": "daveOrg",
      "description": "something something",
      "email": "org@gmail.com",
      "country": "some Country",
      "industry": "tech",
      "location": "lagos",
      "owner_id": "01910544-d1e1-7ada-bdac-c76167489ng7",
      "logo_url": "some url of org logo",
      "channels_count": 3,
      "total_messages_count": 45,
      "org_role": null,
      "created_at": "2024-07-30T21:11:21.9538358+01:00",
      "updated_at": "2024-07-30T21:11:21.9538358+01:00"
    }
  }
  ```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 2. Update an Organization
- **Endpoint:** `PUT /organisations/{orgId}`
- **Summary:** Update an organization
- **Security:** bearerAuth
- **Parameters:**
  - **orgId** (path, required): UUID of the organization to update.
- **Description:** Updates an organization.

#### Request Body
- **Content-Type:** `application/json`
- **Required:** true
```json
{
  "name": "string",
  "description": "string",
  "email": "string",
  "type": "string",
  "location": "string",
  "country": "string",
  "logo_url": "string"
}
```

#### Responses
- **200**: Organization updated successfully.
  ```json
  {
    "status": "success",
    "status_code": 200,
    "message": "Organization updated successfully",
    "data": {
      "id": "01910544-d1e1-7ada-bdac-c761e527ec91",
      "name": "daveOrg",
      "description": "something something",
      "email": "org@gmail.com",
      "country": "some Country",
      "industry": "tech",
      "location": "lagos",
      "owner_id": "01910544-d1e1-7ada-bdac-c76167489ng7",
      "logo_url": "some url of org logo",
      "channels_count": 3,
      "total_messages_count": 45,
      "org_role": null,
      "created_at": "2024-07-30T21:11:21.9538358+01:00",
      "updated_at": "2024-07-30T21:11:21.9538358+01:00"
    }
  }
  ```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 3. Retrieve an Organization
- **Endpoint:** `GET /organisations/{orgId}`
- **Summary:** Retrieve an organization
- **Security:** bearerAuth
- **Parameters:**
  - **orgId** (path, required): UUID of the organization to retrieve.
- **Description:** Retrieve an organization.

#### Responses
- **200**: Organization retrieved successfully.
  ```json
  {
    "status": "success",
    "status_code": 200,
    "message": "Organization retrieved successfully",
    "data": {
      "id": "01910544-d1e1-7ada-bdac-c761e527ec91",
      "name": "daveOrg",
      "description": "something something",
      "email": "org@gmail.com",
      "country": "some Country",
      "industry": "tech",
      "location": "lagos",
      "owner_id": "01910544-d1e1-7ada-bdac-c76167489ng7",
      "logo_url": "some url of org logo",
      "channels_count": 3,
      "total_messages_count": 45,
      "org_role": null,
      "created_at": "2024-07-30T21:11:21.9538358+01:00",
      "updated_at": "2024-07-30T21:11:21.9538358+01:00"
    }
  }
  ```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 4. Delete an Organization
- **Endpoint:** `DELETE /organisations/{orgId}`
- **Summary:** Delete an organization by ID
- **Security:** bearerAuth
- **Parameters:**
  - **orgId** (path, required): UUID of the organization to delete.

#### Responses
- **204**: Organization deleted successfully.
- **400**: Invalid orgId format.
- **401**: Unauthorized.
- **403**: Forbidden.
- **404**: Not found.
- **500**: Internal server error.

---

### 5. Add a User to an Organization
- **Endpoint:** `POST /organisations/{orgId}/users`
- **Summary:** Add a user to an organization by superadmin
- **Security:** bearerAuth
- **Parameters:**
  - **orgId** (path, required): UUID of the organization.

#### Request Body
- **Content-Type:** `application/json`
- **Required:** true
```json
{
  "user_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
  "role_Id": "01910544-d1e1-7ada-bdac-c761e527ec92"
}
```

#### Responses
- **200**: User added successfully.
  ```json
  {
    "status": "success",
    "status_code": 200,
    "message": "User added successfully"
  }
  ```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 6. Get Users in an Organization
- **Endpoint:** `GET /organisations/{orgId}/users`
- **Summary:** Get users in an organization
- **Security:** bearerAuth
- **Parameters:**
  - **orgId** (path, required): UUID of the organization.

#### Responses
- **200**: Users retrieved successfully.
  ```json
  {
    "status": "success",
    "status_code": 200,
    "message": "Users retrieved successfully",
    "data": [
      {
        "id": "01910544-d1e1-7ada-bdac-c761e527ec91",
        "email": "user@gmail.com",
        "phone_number": "09055667788",
        "name": "Iretoms"
      }
    ]
  }
  ```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

Here’s the updated markdown documentation for the additional `/organisations` endpoints, including the schemas you provided for fetching user channels and archived channels:

### 7. Fetch User Channels
- **Endpoint:** `GET /organisations/:org_id/user-channels`
- **Summary:** Fetch User Channels
- **Description:** Retrieve the list of channels associated with the authenticated user.

#### Responses
- **200**: A list of channels associated with the user.
  ```json
  {
    "status": "success",
    "status_code": 200,
    "message": "User channels fetched successfully",
    "data": [
      {
        "id": "0191943c-d263-71f2-99d5-82ae6afe2da4",
        "name": "Default",
        "description": "shallom's org's default channel",
        "created_at": "2024-08-27T15:28:20.499875+01:00"
      }
    ]
  }
  ```

---

### 8. Fetch Channels User Does Not Belong To
- **Endpoint:** `GET /organisations/{organisation_id}/user-not-channels`
- **Summary:** Fetch Channels User Does Not Belong To
- **Description:** Retrieves a list of channels within the specified organization that the user does not belong to.
- **Parameters:**
  - **organisation_id** (path, required): Unique identifier for the organization.

#### Responses
- **200**: Channels user does not belong to fetched successfully.
  ```json
  {
    "status": "success",
    "status_code": 200,
    "message": "Channels user does not belong fetched successfully",
    "data": [
      {
        "id": "01919b40-4f41-7154-b2b2-66341ce641ec",
        "name": "programmers",
        "description": "this room is meant for programmers",
        "created_at": "2024-08-29T00:09:29.595066+01:00"
      }
    ]
  }
  ```

---

### 9. Retrieve Archived Channels
- **Endpoint:** `GET /organisations/{organisation_id}/channels/archived`
- **Summary:** Retrieve archived channels
- **Description:** Fetch all archived channels for a specific organization.
- **Parameters:**
  - **organisation_id** (path, required): ID of the organization whose archived channels are to be retrieved.

#### Responses
- **200**: Successfully retrieved archived channels.
  ```json
  {
    "status": "success",
    "status_code": 200,
    "message": "Archived channels retrieved successfully",
    "data": [
      {
        "channels_id": "0191be6e-4742-7b48-b3fd-437e8f5452ad",
        "name": "Default",
        "description": "shallom's org's default channel",
        "organisation_id": "0191be6e-473e-7b48-888f-810f4c449dc9",
        "owner_id": "0191be6d-4e1b-7b47-8f92-4f009762cfaa",
        "users": null,
        "user_count": 0,
        "message_count": 0,
        "archived": true,
        "group_id": null,
        "created_at": "2024-09-04T20:06:24.257395+01:00",
        "deleted_at": "0001-01-01T00:13:35+00:13",
        "threads": null
      }
    ]
  }
  ```
