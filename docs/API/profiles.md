# Profiles

## Overview
The **Telex Profiles API** provides endpoints to manage user profiles within Telex. This includes retrieving the user profile, updating profile information, and deleting the user profile image. These endpoints ensure efficient handling of profile operations and data integration.

### 1. Get User Profile
- **Endpoint:** `GET /profile`
- **Tags:** Profile
- **Summary:** Retrieve user profile
- **Security:** 
  - `bearerAuth: []`

#### Responses
- **200**: User profile retrieved successfully.
```json
{
  "status": "success",
  "message": "User profile retrieved successfully",
  "statusCode": 200,
  "data": {
    "id": "019142c8-5408-7a48-8173-f31e6c8e00cd",
    "email": "keepStrugle@gmail.com",
    "phone": "09099999999",
    "first_name": "Test",
    "last_name": "Og",
    "full_name": "Testinp Update",
    "username": "What_b_Heck",
    "avatar_url": "img898989.png",
    "userId": "019142c8-5408-7a47-b94c-91d674dd8358",
    "created_at": "2024-08-11T19:51:50+01:00",
    "updated_at": "2024-08-12T00:59:39+01:00",
    "deleted_at": "0001-01-01T00:00:00Z"
  }
}
```
- **401**: Unauthenticated.
```json
{
  "status": "error",
  "status_code": 401,
  "message": "Unauthorized access"
}
```
- **500**: Server error.
```json
{
  "status": "error",
  "status_code": 500,
  "message": "Internal server error"
}
```

---

### 2. Update User Profile
- **Endpoint:** `PATCH /profile`
- **Tags:** Profile
- **Summary:** Update user profile
- **Description:** Allows authenticated users to update their profile information.
- **Security:** 
  - `bearerAuth: []`

#### Request Body
```json
{
  "email": "newemail@example.com",
  "full_name": "New Full Name",
  "username": "new_username",
  "phone": "1234567890",
  "avatar_url": "https://example.com/avatar.jpg"
}
```

#### Responses
- **200**: Profile updated successfully.
```json
{
  "status": "success",
  "message": "Profile updated successfully",
  "status_code": 200,
  "data": {
    "email": "newemail@example.com",
    "full_name": "New Full Name",
    "username": "new_username",
    "phone": "1234567890",
    "avatar_url": "https://example.com/avatar.jpg"
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
- **500**: Server error.
```json
{
  "status": "error",
  "status_code": 500,
  "message": "Internal server error"
}
```

---

### 3. Delete User Profile Image
- **Endpoint:** `DELETE /profile/image`
- **Tags:** Profile
- **Summary:** Delete user profile image
- **Description:** Allows authenticated users to delete their profile image.
- **Security:** 
  - `bearerAuth: []`

#### Responses
- **200**: Profile image deleted successfully.
```json
{
  "status": "success",
  "message": "Profile image deleted successfully",
  "status_code": 200
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
- **500**: Server error.
```json
{
  "status": "error",
  "status_code": 500,
  "message": "Internal server error"
}
```

---

## Organization Invitation API Documentation

### 4. Create and Send Invitations
- **Endpoint:** `POST /invite`
- **Tags:** Organisation-Invitation
- **Summary:** Create and send invitations
- **Description:** Sends invitations to the specified email addresses to join the organization.

#### Request Body
```json
{
  "org_id": "01918214-55e5-791b-9ebd-fa8d92d69fa1",
  "emails": ["micahshallom@gmail.com", "micahshallom24@gmail.com"],
  "role": "member"
}
```

#### Responses
- **201**: Invitations created and sent successfully.
```json
{
  "status": "success",
  "status_code": 201,
  "message": "Invitations created successfully",
  "data": {
    "errors": [
      "An invitation has already been sent to micahshallom@gmail.com",
      "Error checking for telex presence: user with email micahshallom24@gmail.com does not exist"
    ],
    "invitations": [
      {
        "id": "019185b0-8fb4-767f-bd9d-d612e588f465",
        "email": "micahshallom24@gmail.com",
        "org_id": "01918214-55e5-791b-9ebd-fa8d92d69fa1",
        "status": "invited",
        "invite_token": "a351f47cea15f91ca16a3b18a04a8c22",
        "is_telex_user": false,
        "invitation_link": "http://127.0.0.1:8019/api/v1/invite/accept_org_invitation?org_id=01918214-55e5-791b-9ebd-fa8d92d69fa1&invitation_token=a351f47cea15f91ca16a3b18a04a8c22",
        "sent_at": "2024-08-24T19:40:26.548569958+01:00",
        "expires_at": "2024-08-26T19:40:26.548423408+01:00"
      }
      // More invitation objects...
    ]
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
- **500**: Server error.
```json
{
  "status": "error",
  "status_code": 500,
  "message": "Internal server error"
}
```