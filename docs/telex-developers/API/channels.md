# Channels

## Overview
The **Telex Channels API** provides a robust set of endpoints to manage channels within your application. This includes creating, updating, retrieving, and deleting channels, as well as managing users within a channel. These endpoints are designed to integrate seamlessly into your application, ensuring efficient and effective channel management.

The API supports features like user association, message management, and channel metadata management, enabling developers to implement advanced channel management flows with minimal effort.

### Create a New Channel
- **Endpoint:** `POST /channels`
- **Tags:** channels
- **Summary:** Create a new channel
- **Security:** bearerAuth
- **Description:** Creates a new channel.

#### Request Body
- **Content-Type:** `application/json`
- **Required:** true
```json
{
    "name": "General",
    "description": "General discussion",
    "userIds": ["01910544-d1e1-7ada-bdac-c761e527ec91"],
    "organisation_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
    "username": "creator_username"
}
```

#### Responses
- **201**: Channel created successfully.
```json
{
    "status": "success",
    "status_code": 201,
    "message": "Channel created successfully",
    "data": {
        "id": "01910544-d1e1-7ada-bdac-c761e527ec91",
        "name": "General",
        "description": "General discussion",
        "is_private": false,
        "is_dm": false,
        "organisation_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
        "created_by": "01910544-d1e1-7ada-bdac-c761e527ec91",
        "created_at": "2024-07-30T21:11:21.9538358+01:00",
        "updated_at": "2024-07-30T21:11:21.9538358+01:00"
    }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### Retrieve a List of Channels
- **Endpoint:** `GET /channels`
- **Tags:** channels
- **Summary:** Retrieve a list of channels
- **Security:** bearerAuth
- **Description:** Retrieves a list of channels.

#### Responses
- **200**: Channels retrieved successfully.
```json
{
    "channels": [
        {
            "channels_id": "019146f9-68f3-7294-9c4e-a158cc0f7e3a",
            "name": "hackers",
            "description": "this room is meant for hackers",
            "organisation_id": "019146f9-3d17-7294-93ac-9963ada4b7c1",
            "owner_id": "019146f6-fd9b-7293-a5d2-b05ba6be2185",
            "users": ["01910544-d1e1-7ada-bdac-c761e527ec91"],
            "user_count": 1,
            "message_count": 0,
            "webhook_url": "https://ping.staging.telex.im/v1/webhooks/someid",
            "created_at": "2024-08-12T15:23:56.147736+01:00",
            "deleted_at": "0001-01-01T00:13:35+00:13",
            "messages": []
        }
    ]
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### Create a New Message in a Channel
- **Endpoint:** `POST /channels/{channel_id}/messages`
- **Tags:** messages
- **Summary:** Create a new message
- **Security:** bearerAuth
- **Parameters:**
    - **channel_id** (path, required): ID of the channel to create the message in.

#### Request Body
- **Content-Type:** `application/json`
- **Required:** true
```json
{
    "content": "Hello, world!"
}
```

#### Responses
- **201**: Message created successfully.
```json
{
    "status": "success",
    "status_code": 201,
    "message": "Message created successfully",
    "data": {
        "id": "message_id",
        "content": "Hello, world!",
        "channel_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
        "created_at": "2024-07-30T21:11:21.9538358+01:00"
    }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### Edit a Message in a Channel
- **Endpoint:** `PUT /channels/{channel_id}/messages`
- **Tags:** messages
- **Summary:** Edit a message
- **Security:** bearerAuth
- **Parameters:**
    - **channel_id** (path, required): ID of the channel to edit the message in.

#### Request Body
- **Content-Type:** `application/json`
- **Required:** true
```json
{
    "content": "Updated message content"
}
```

#### Responses
- **200**: Message updated successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Message updated successfully",
    "data": {
        "id": "message_id",
        "content": "Updated message content",
        "channel_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
        "updated_at": "2024-07-30T21:11:21.9538358+01:00"
    }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### Retrieve a List of Messages in a Channel
- **Endpoint:** `GET /channels/{channel_id}/messages`
- **Tags:** messages
- **Summary:** Retrieve a list of messages
- **Security:** bearerAuth
- **Parameters:**
    - **channel_id** (path, required): ID of the channel to retrieve messages for.

#### Responses
- **200**: Messages retrieved successfully.
```json
{
    "messages": [
        {
            "id": "message_id",
            "content": "Hello, world!",
            "created_at": "2024-07-30T21:11:21.9538358+01:00"
        }
    ]
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.

---

### 6. Retrieve a Specific Channel by Name
- **Endpoint:** `GET /channels/name/{channelName}`
- **Tags:** channels
- **Summary:** Retrieve a specific channel by name
- **Security:** bearerAuth
- **Parameters:**
  - **channelName** (path, required): Name of the channel to retrieve.

#### Responses
- **200**: Channel retrieved successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "channel name retrieved successfully",
  "data": {
    "channels_id": "019146f9-68f3-7294-9c4e-a158cc0f7e3a",
    "name": "hackers",
    "description": "this room is meant for hackers",
    "organisation_id": "019146f9-3d17-7294-93ac-9963ada4b7c1",
    "owner_id": "019146f6-fd9b-7293-a5d2-b05ba6be2185",
    "users": ["01910544-d1e1-7ada-bdac-c761e527ec91"],
    "created_at": "2024-08-12T15:23:56.147736+01:00"
  }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 7. Join a Channel
- **Endpoint:** `POST /channels/{channelId}/join`
- **Tags:** channels
- **Summary:** Join a channel
- **Security:** bearerAuth
- **Parameters:**
  - **channelId** (path, required): ID of the channel to join.

#### Responses
- **200**: Channel joined successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "User joined channel successfully",
  "data": {
    "name": "General",
    "description": "General discussion",
    "userId": "01910544-d1e1-7ada-bdac-c761e527ec91",
    "channel_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
    "organisation_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
    "owner_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
    "created_at": "2024-07-30T21:11:21.9538358+01:00"
  }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 8. Leave a Channel
- **Endpoint:** `POST /channels/{channelId}/leave`
- **Tags:** channels
- **Summary:** Leave a channel
- **Security:** bearerAuth
- **Parameters:**
  - **channelId** (path, required): ID of the channel to leave.

#### Responses
- **200**: Channel left successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "User left channel successfully",
  "data": {
    "name": "General",
    "description": "General discussion",
    "userId": "01910544-d1e1-7ada-bdac-c761e527ec91",
    "channel_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
    "organisation_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
    "owner_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
    "created_at": "2024-07-30T21:11:21.9538358+01:00"
  }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 9. Update a Channel's Username
- **Endpoint:** `PATCH /channels/{channelId}/username`
- **Tags:** channels
- **Summary:** Update a channel's username
- **Security:** bearerAuth
- **Parameters:**
  - **channelId** (path, required): ID of the channel to update the username for.

#### Request Body
- **Content-Type:** `application/json`
- **Required:** true
```json
{
  "username": "new_channel_username"
}
```

#### Responses
- **200**: Channel username updated successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Username updated successfully"
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 10. Retrieve a Specific Channel
- **Endpoint:** `GET /channels/{channelId}`
- **Tags:** channels
- **Summary:** Retrieve a specific channel
- **Security:** bearerAuth
- **Parameters:**
  - **channelId** (path, required): ID of the channel to retrieve.

#### Responses
- **200**: Channel retrieved successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "channel retrieved successfully",
  "data": {
    "channels_id": "019146f9-68f3-7294-9c4e-a158cc0f7e3a",
    "name": "hackers",
    "description": "this room is meant for hackers",
    "organisation_id": "019146f9-3d17-7294-93ac-9963ada4b7c1",
    "owner_id": "019146f6-fd9b-7293-a5d2-b05ba6be2185",
    "users": ["01910544-d1e1-7ada-bdac-c761e527ec91"],
    "created_at": "2024-08-12T15:23:56.147736+01:00"
  }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 11. Update a Channel
- **Endpoint:** `PATCH /channels/{channelId}`
- **Tags:** channels
- **Summary:** Update a channel
- **Security:** bearerAuth
- **Parameters:**
  - **channelId** (path, required): ID of the channel to update.

#### Request Body
- **Content-Type:** `application/json`
- **Required:** true
```json
{
  "name": "Updated Channel Name",
  "description": "Updated description of the channel"
}
```

#### Responses
- **200**: Channel updated successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Channel updated successfully",
  "data": {
    "channels_id": "019146f9-68f3-7294-9c4e-a158cc0f7e3a",
    "name": "Updated Channel Name",
    "description": "Updated description of the channel",
    "organisation_id": "019146f9-3d17-7294-93ac-9963ada4b7c1",
    "owner_id": "019146f6-fd9b-7293-a5d2-b05ba6be2185",
    "created_at": "2024-08-12T15:23:56.147736+01:00"
  }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 12. Delete a Channel
- **Endpoint:** `DELETE /channels/{channelId}`
- **Tags:** channels
- **Summary:** Delete a channel
- **Security:** bearerAuth
- **Parameters:**
  - **channelId** (path, required): ID of the channel to delete.

#### Responses
- **204**: Channel deleted successfully.
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **404**: Not found.
- **500**: Server error.

---

### 13. Check if a User Exists in a Channel
- **Endpoint:** `GET /channels/{channelId}/user-exist`
- **Tags:** channels
- **Summary:** Check if a user exists in a channel
- **Security:** bearerAuth
- **Parameters:**
  - **channelId** (path, required): ID of the channel to check for the user.
  - **userId** (query, required): ID of the user to check for.

#### Responses
- **200**: User exists in channel.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "User exists in channel",
  "data": {
    "exists": true
  }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 14. Get All Channels in an Organisation
- **Endpoint:** `GET /organisations/{organisationId}/channels`
- **Tags:** channels
- **Summary:** Get all channels in an organisation
- **Security:** bearerAuth
- **Parameters:**
  - **organisationId** (path, required): ID of the organisation to check for the channel.

#### Responses
- **200**: Get all channels in an organisation.
```json
{
  "channels": [
    {
      "channels_id": "019146f9-68f3-7294-9c4e-a158cc0f7e3a",
      "name": "hackers",
      "description": "this room is meant for hackers",
      "organisation_id": "019146f9-3d17-7294-93ac-9963ada4b7c1",
      "owner_id": "019146f6-fd9b-7293-a5d2-b05ba6be2185",
      "users": ["01910544-d1e1-7ada-bdac-c761e527ec91"],
      "created_at": "2024-08-12T15:23:56.147736+01:00"
    }
  ]
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 15. Retrieve the Number of Users in a Channel
- **Endpoint:** `GET /channels/{channelId}/num-users`
- **Tags:** channels
- **Summary:** Retrieve the number of users in a channel
- **Security:** bearerAuth
- **Parameters:**
  - **channelId** (path, required): ID of the channel to retrieve the number of users for.

#### Responses
- **200**: Number of users retrieved successfully.
```json
{
  "user_count": 5
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.

---

### 16. Retrieve Users in a Channel
- **Endpoint:** `GET /channels/{channelId}/users`
- **Tags:** channels
- **Summary:** Retrieve users in a channel
- **Security:** bearerAuth
- **Parameters:**
  - **channelId** (path, required): ID of the channel to retrieve users from.

#### Responses
- **200**: Users retrieved successfully.
```json
{
  "users": [
    {
      "user_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
      "username": "user1",
      "joined_at": "2024-07-30T21:11:21.9538358+01:00"
    },
    {
      "user_id": "01910544-d1e1-7ada-bdac-c761e527ec92",
      "username": "user2",
      "joined_at": "2024-07-30T21:11:21.9538358+01:00"
    }
  ]
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **404**: Not found.
- **500**: Server error.
