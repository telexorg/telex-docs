# Threads

## Overview
The **Telex Threads API** provides endpoints to manage threads within Telex. This includes retrieving all threads for a user in a specific organization, retrieving all threads in a specific channel, retrieving a specific thread's messages, updating the status of a specific thread, and posting thread messages. These endpoints ensure efficient handling of thread operations and data integration.

Here’s the updated documentation for the first endpoint, incorporating the new thread object structure you provided.

---

### Retrieve All Threads for a User in a Specific Organization
- **Endpoint:** `GET /threads/organisations/{orgId}`
- **Tags:** threads
- **Summary:** Retrieve all threads for a user in a specific organization
- **Security:** 
    - `bearerAuth: []`

#### Parameters
- **orgId**: The ID of the organization (path parameter, required, UUID).

#### Responses
- **200**: Threads retrieved successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Data retrieved successfully",
    "data": [
        {
            "thread_id": "01916ea9-a37d-7b73-828c-f54f03ba3243",
            "channels_id": "01916476-b76b-7588-b9f5-2e8094088dc3",
            "event_name": "strawhat",
            "username": "cyberguru",
            "action_type": "some action for guru",
            "status": "success",
            "created_at": "2024-08-20T02:21:36.893485-05:00",
            "messages": [
                {
                    "id": "3bcc7d41-a960-4006-b9fc-923a241a9003",
                    "content": "some content2",
                    "channels_id": "01916476-b76b-7588-b9f5-2e8094088dc3",
                    "user_id": "01916474-ab82-7587-ae01-847d103b7ee3",
                    "username": "cyberguru",
                    "created_at": "2024-09-27T10:45:24.361-05:00",
                    "updated_at": "2024-08-22T07:38:57.922988Z",
                    "thread_id": "01916b4d-babc-7aa9-a430-c420e3290578",
                    "mentions": null,
                    "avatar_url": "http://91.229.239.238:7100/telex-dev-bucket/public/profile_pics/profile_pic_18ff64dd6692.jpeg"
                }
            ],
            "message_count": 0,
            "last_reply": "0001-01-01T00:00:00Z",
            "avatar_url": "",
            "type": "",
            "content": "",
            "channel_name": "",
            "current_status": ""
        }
    ]
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
    "message": "Organization not found"
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

### Retrieve All Threads in a Specific Channel
- **Endpoint:** `GET /threads/channels/{channelId}`
- **Tags:** threads
- **Summary:** Retrieve all threads (thread and message) in a specific channel
- **Security:** 
    - `bearerAuth: []`

#### Parameters
- **channelId**: The ID of the channel (path parameter, required).

#### Responses
- **200**: Threads retrieved successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Data retrieved successfully",
    "data": [
        {
            "thread_id": "uuid",
            "channels_id": "uuid",
            "event_name": "Event Name",
            "username": "test",
            "action_type": "",
            "status": "success",
            "created_at": "2024-08-24T08:59:13.253151-05:00",
            "messages": [
                {
                    "id": "uuid",
                    "content": "test",
                    "channels_id": "uuid",
                    "user_id": "uuid",
                    "username": "cyber",
                    "created_at": "2024-08-24T08:02:44.089-05:00",
                    "updated_at": "2024-08-27T02:51:40.078425Z",
                    "thread_id": "uuid",
                    "mentions": null,
                    "avatar_url": "http://profile-link.com"
                }
            ],
            "message_count": 0,
            "last_reply": "0001-01-01T00:00:00Z",
            "avatar_url": "",
            "type": "thread",
            "content": "test",
            "channel_name": "",
            "current_status": "",
            "full_name
