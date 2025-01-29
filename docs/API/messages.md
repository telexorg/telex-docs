# Messages

## Overview
The **Telex Messages API** provides endpoints to manage messages within telex. This includes creating, editing, and retrieving messages within a channel. These endpoints ensure efficient handling of message operations and data integration.

### 1. Create a New Message
- **Endpoint:** `POST /channels/{channel_id}/messages`
- **Tags:** messages
- **Summary:** Create a new message
- **Security:** 
  - `bearerAuth: []`
- **Description:** Creates a new message within a channel.

#### Parameters
- **channel_id**: The ID of the channel (path parameter, required, UUID).

#### Request Body
```json
{
  "content": "Your message content here",
  "thread_id": "optional-thread-id" // Optional if the message is part of a thread
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
    "id": "uuid",
    "content": "Your message content here",
    "channel_id": "uuid",
    "created_at": "2023-01-01T00:00:00Z",
    "updated_at": "2023-01-01T00:00:00Z"
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
- **422**: Validation error.
```json
{
  "status": "error",
  "status_code": 422,
  "message": "Validation error",
  "errors": ["Field errors"]
}
```

---

### 2. Edit a Message
- **Endpoint:** `PUT /channels/{channel_id}/messages`
- **Tags:** messages
- **Summary:** Edit a message
- **Security:** 
  - `bearerAuth: []`
- **Description:** Edit a message within a thread.

#### Parameters
- **channel_id**: The ID of the channel (path parameter, required, UUID).

#### Request Body
```json
{
  "content": "Updated message content",
  "thread_id": "optional-thread-id" // Optional if editing a thread message
}
```

#### Responses
- **201**: Message updated successfully.
```json
{
  "status": "success",
  "status_code": 201,
  "message": "Message updated successfully",
  "data": {
    "id": "uuid",
    "content": "Updated message content",
    "channel_id": "uuid",
    "created_at": "2023-01-01T00:00:00Z",
    "updated_at": "2023-01-01T00:00:00Z"
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
- **422**: Validation error.
```json
{
  "status": "error",
  "status_code": 422,
  "message": "Validation error",
  "errors": ["Field errors"]
}
```

---

### 3. Retrieve a List of Messages
- **Endpoint:** `GET /channels/{channel_id}/messages`
- **Tags:** messages
- **Summary:** Retrieve a list of messages
- **Security:** 
  - `bearerAuth: []`
- **Description:** Retrieves a list of messages within a channel.

#### Parameters
- **channel_id**: The ID of the channel (path parameter, required, UUID).

#### Responses
- **200**: Messages retrieved successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Messages retrieved successfully",
  "data": {
    "messages": [
      {
        "id": "uuid",
        "content": "Message content here",
        "channel_id": "uuid",
        "created_at": "2023-01-01T00:00:00Z",
        "updated_at": "2023-01-01T00:00:00Z"
      }
      // More message objects...
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
- **401**: Unauthorized.
```json
{
  "status": "error",
  "status_code": 401,
  "message": "Unauthorized access"
}
```
- **404**: Not found.
```json
{
  "status": "error",
  "status_code": 404,
  "message": "Channel not found"
}
```