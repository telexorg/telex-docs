# Webhooks

## Overview
The **Telex Webhooks API** provides endpoints to manage webhooks within your application. This includes creating, updating, retrieving, and deleting webhooks, as well as managing webhook statuses and histories. These endpoints ensure efficient handling of webhook events and data integration.

### 1. Retrieve a Channel's Webhook
- **Endpoint:** `GET /webhooks/{channel_id}`
- **Tags:** webhooks
- **Summary:** Retrieve a channel's webhook
- **Security:** 
  - `bearerAuth: []`
  
#### Parameters
- **channel_id**: The ID of the channel (path parameter, required, UUID).

#### Responses
- **200**: Channel webhook retrieved successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Webhook fetched successfully",
  "data": {
    "id": "uuid",
    "event_name": "Event Name",
    "webhook_name": "Webhook Name",
    "status": "active",
    "owner_id": "uuid",
    "webhook_url": "https://example.com/webhook",
    "webhook_slug": "webhook-slug",
    "channel_id": "uuid",
    "created_at": "2023-01-01T00:00:00Z",
    "deleted_at": null,
    "updated_at": "2023-01-01T00:00:00Z"
  }
}
```

---

### 2. Create a New Webhook
- **Endpoint:** `POST /webhooks/{channel_id}`
- **Tags:** webhooks
- **Summary:** Create a new webhook
- **Security:** 
  - `bearerAuth: []`

#### Parameters
- **channel_id**: The ID of the channel (path parameter, required, UUID).

#### Request Body
```json
{
  "webhook_name": "New Webhook Name",
  "event_name": "New Event Name",
  "webhook_url": "https://example.com/webhook"
}
```

#### Responses
- **201**: Webhook created successfully.
```json
{
  "status": "success",
  "status_code": 201,
  "message": "Webhook created successfully",
  "data": {
    "id": "uuid",
    "event_name": "New Event Name",
    "webhook_name": "New Webhook Name",
    "status": "active",
    "owner_id": "uuid",
    "webhook_url": "https://example.com/webhook",
    "webhook_slug": "webhook-slug",
    "channel_id": "uuid",
    "created_at": "2023-01-01T00:00:00Z",
    "deleted_at": null,
    "updated_at": "2023-01-01T00:00:00Z"
  }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 3. List All Webhooks
- **Endpoint:** `GET /webhooks/{channel_id}/all`
- **Tags:** webhooks
- **Summary:** List all webhooks
- **Security:** 
  - `bearerAuth: []`

#### Parameters
- **channel_id**: The ID of the channel (path parameter, required, UUID).
- **page**: The page number to retrieve (query parameter, optional, default: 1).
- **limit**: The number of items per page (query parameter, optional, default: 10).

#### Responses
- **200**: A list of webhooks retrieved successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Webhooks fetched successfully",
  "data": {
    "webhooks": [
      {
        "id": "uuid",
        "event_name": "Event Name",
        "webhook_name": "Webhook Name",
        "status": "active",
        "owner_id": "uuid",
        "webhook_url": "https://example.com/webhook",
        "webhook_slug": "webhook-slug",
        "channel_id": "uuid",
        "created_at": "2023-01-01T00:00:00Z",
        "deleted_at": null,
        "updated_at": "2023-01-01T00:00:00Z"
      }
      // More webhook objects...
    ]
  }
}
```

---

### 4. Get a Webhook History by ID
- **Endpoint:** `GET /webhooks/{channel_id}/history/{webhook_id}`
- **Tags:** webhooks
- **Summary:** Get a webhook history by ID
- **Security:** 
  - `bearerAuth: []`

#### Parameters
- **channel_id**: The ID of the channel (path parameter, required, UUID).
- **webhook_id**: The ID of the webhook (path parameter, required, UUID).
- **page**: The page number to retrieve (query parameter, optional, default: 1).
- **limit**: The number of items per page (query parameter, optional, default: 10).

#### Responses
- **200**: Webhook history retrieved successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Webhook history fetched successfully",
  "data": {
    "webhooks_history": [
      {
        "id": "uuid",
        "webhook_id": "uuid",
        "callback_id": "uuid",
        "status_code": "200",
        "action_type": "created",
        "retries": "0",
        "attempted": "2023-01-01T00:00:00Z"
      }
      // More webhook history objects...
    ]
  }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 5. Update a Webhook
- **Endpoint:** `PUT /webhooks/{channel_id}/{webhook_id}`
- **Tags:** webhooks
- **Summary:** Update a webhook
- **Security:** 
  - `bearerAuth: []`

#### Parameters
- **channel_id**: The ID of the channel (path parameter, required, UUID).
- **webhook_id**: The ID of the webhook (path parameter, required).

#### Request Body
```json
{
  "webhook_name": "Updated Webhook Name",
  "event_name": "Updated Event Name"
}
```

#### Responses
- **200**: Webhook updated successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Webhook updated successfully",
  "data": {
    "id": "uuid",
    "event_name": "Updated Event Name",
    "webhook_name": "Updated Webhook Name",
    "status": "active",
    "owner_id": "uuid",
    "webhook_url": "https://example.com/webhook",
    "webhook_slug": "webhook-slug",
    "channel_id": "uuid",
    "created_at": "2023-01-01T00:00:00Z",
    "deleted_at": null,
    "updated_at": "2023-01-01T00:00:00Z"
  }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 6. Delete a Webhook
- **Endpoint:** `DELETE /webhooks/{channel_id}/{webhook_id}`
- **Tags:** webhooks
- **Summary:** Delete a webhook
- **Security:** 
  - `bearerAuth: []`

#### Parameters
- **channel_id**: The ID of the channel (path parameter, required, UUID).
- **webhook_id**: The ID of the webhook (path parameter, required).

#### Responses
- **204**: Webhook deleted successfully.
```json
{
  "status": "success",
  "status_code": 204,
  "message": "Webhook deleted successfully"
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 7. Update Webhook Status
- **Endpoint:** `PUT /webhooks/{channel_id}/{webhook_id}/change-status`
- **Tags:** webhooks
- **Summary:** Update webhook status
- **Security:** 
  - `bearerAuth: []`

#### Parameters
- **channel_id**: The ID of the channel (path parameter, required, UUID).
- **webhook_id**: The ID of the webhook (path parameter, required).

#### Request Body
```json
{
  "webhook_status": "active" // Can be "active", "inactive", or "paused"
}
```

#### Responses
- **200**: Webhook status updated successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Webhook updated successfully",
  "data": {
    "id": "uuid",
    "webhook_status": "active"
  }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 8. Send Data to Webhook (Backend Queue)
- **Endpoint:** `POST /webhooks/feed/backend-queue`
- **Tags:** Feed_Webhook
- **Summary:** Send data to webhook
- **Security:** 
  - `bearerAuth: []`

#### Request Body
```json
{
  "channel_id": "uuid",
  "event_name": "Event Name",
  "message": "Message Content",
  "status": "Status",
  "username": "Username"
}
```

#### Responses
- **200**: Data received successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Data sent successfully",
  "data": null
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 9. Send Data to Webhook (Feed)
- **Endpoint:** `POST /webhooks/feed/{webhook_slug}`
- **Tags:** Feed_Webhook
- **Summary:** Send data to webhook
- **Security:** 
  - `bearerAuth: []`

#### Parameters
- **webhook_slug**: The webhook slug (path parameter, required).

#### Request Body
```json
{
  "event_name": "Event Name",
  "message": "Message Content",
  "status": "Status",
  "username": "Username"
}
```

#### Responses
- **200**: Data received successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Data sent successfully",
  "data": null
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### 10. Get Data from Webhook (Feed)
- **Endpoint:** `GET /webhooks/feed/{webhook_slug}`
- **Tags:** Feed_Webhook
- **Summary:** Get data from webhook
- **Security:** 
  - `bearerAuth: []`

#### Parameters
- **webhook_slug**: The webhook slug (path parameter, required).
- **username**: The username (query parameter, required).
- **event_name**: The event name (query parameter, required).
- **message**: The message (query parameter, required).
- **status**: The status (query parameter, required).

#### Responses
- **200**: Data received successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Data sent successfully",
  "data": null
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

