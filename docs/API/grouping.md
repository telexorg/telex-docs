
# Channel Grouping

## Overview
The **Telex Channel Grouping API** provides endpoints to manage groups and channels within an organization in Telex. This includes creating, updating, retrieving, and deleting groups, as well as assigning and managing channels within those groups. These endpoints ensure efficient handling of group and channel operations and data integration.

## Create a New Group
- **Endpoint:** `/organisations/{organisation_id}/groups`
- **Method:** `POST`
- **Summary:** Create a new group.

### Parameters
- **organisation_id** (path): ID of the organisation.

### Request Body
```json
{
    "name": "C-Group",
    "description": "This is a group for the C warriors"
}
```

### Responses
- **200**: Group created successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Group created successfully",
    "data": {
        "id": "unique_group_id",
        "name": "C-Group",
        "description": "This is a group for the C warriors",
        "organisation_id": "organisation_id",
        "channels": null,
        "archived": false,
        "created_at": "timestamp",
        "updated_at": "timestamp",
        "deleted_at": null
    }
}
```

---

## Fetch All Groups
- **Endpoint:** `/organisations/{organisation_id}/groups`
- **Method:** `GET`
- **Summary:** Fetch all groups.

### Parameters
- **organisation_id** (path): ID of the organisation.

### Responses
- **200**: Groups fetched successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Groups fetched successfully",
    "data": [
        {
            "id": "unique_group_id",
            "name": "C-Group",
            "description": "This is a group for the C warriors",
            "organisation_id": "organisation_id",
            "channels": null,
            "archived": false,
            "created_at": "timestamp",
            "updated_at": "timestamp",
            "deleted_at": null
        }
        // More group objects...
    ]
}
```

---

## Fetch a Specific Group
- **Endpoint:** `/organisations/{organisation_id}/groups/{group_id}`
- **Method:** `GET`
- **Summary:** Fetch a specific group.

### Parameters
- **organisation_id** (path): ID of the organisation.
- **group_id** (path): ID of the group.

### Responses
- **200**: Group fetched successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Group fetched successfully",
    "data": {
        "id": "unique_group_id",
        "name": "C-Group",
        "description": "This is a group for the C warriors",
        "organisation_id": "organisation_id",
        "channels": null,
        "archived": false,
        "created_at": "timestamp",
        "updated_at": "timestamp",
        "deleted_at": null
    }
}
```

---

## Update a Group
- **Endpoint:** `/organisations/{organisation_id}/groups/{group_id}`
- **Method:** `PATCH`
- **Summary:** Update a group.

### Parameters
- **organisation_id** (path): ID of the organisation.
- **group_id** (path): ID of the group.

### Request Body
```json
{
    "name": "Updated Group Name",
    "description": "Updated description"
}
```

### Responses
- **200**: Group updated successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Group updated successfully",
    "data": {
        "id": "unique_group_id",
        "name": "Updated Group Name",
        "description": "Updated description",
        "organisation_id": "organisation_id",
        "channels": null,
        "created_at": "timestamp",
        "updated_at": "timestamp",
        "deleted_at": null
    }
}
```

---

## Delete a Group
- **Endpoint:** `/organisations/{organisation_id}/groups/{group_id}`
- **Method:** `DELETE`
- **Summary:** Delete a group.

### Parameters
- **organisation_id** (path): ID of the organisation.
- **group_id** (path): ID of the group.

### Responses
- **200**: Group deleted successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Group deleted successfully"
}
```

---

## Assign a Channel to a Group
- **Endpoint:** `/organisations/{organisation_id}/groups/{group_id}/channels/{channel_id}`
- **Method:** `POST`
- **Summary:** Assign a specified channel to a group within an organisation.

### Parameters
- **organisation_id** (path): ID of the organisation.
- **group_id** (path): ID of the group.
- **channel_id** (path): ID of the channel.

### Responses
- **200**: Successfully assigned channel to group.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Group channel assigned successfully"
}
```

---

## Remove a Channel from a Group
- **Endpoint:** `/organisations/{organisation_id}/groups/{group_id}/channels/{channel_id}`
- **Method:** `DELETE`
- **Summary:** Remove a specified channel from a group within an organisation.

### Parameters
- **organisation_id** (path): ID of the organisation.
- **group_id** (path): ID of the group.
- **channel_id** (path): ID of the channel.

### Responses
- **200**: Successfully removed channel from group.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Group channel removed successfully"
}
```

---

## Move a Channel to a Different Group
- **Endpoint:** `/organisations/{organisation_id}/groups/{group_id}/channels/{channel_id}`
- **Method:** `PATCH`
- **Summary:** Move a specified channel from one group to another within the same organisation.

### Parameters
- **organisation_id** (path): ID of the organisation.
- **group_id** (path): ID of the new group to move the channel to.
- **channel_id** (path): ID of the channel to be moved.

### Responses
- **200**: Successfully moved the channel to a new group.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Group channel moved successfully"
}
```

---

## Fetch All Channels in a Group
- **Endpoint:** `/organisations/{organisation_id}/groups/{group_id}/channels`
- **Method:** `GET`
- **Summary:** Retrieve a list of channels assigned to a specified group.

### Parameters
- **organisation_id** (path): ID of the organisation.
- **group_id** (path): ID of the group.

### Responses
- **200**: Successfully fetched group channels.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Group channels fetched successfully",
    "data": [
        {
            "channels_id": "unique_channel_id",
            "name": "Channel-4",
            "description": "Channel-4",
            "organisation_id": "organisation_id",
            "owner_id": "owner_id",
            "users": null,
            "user_count": 0,
            "message_count": 0,
            "archived": false,
            "group_id": "group_id",
            "created_at": "timestamp",
            "deleted_at": null
        }
        // More channel objects...
    ]
}
```

---

## Fetch Channels Not in Any Group
- **Endpoint:** `/organisations/{organisation_id}/groups/channels/no-group`
- **Method:** `GET`
- **Summary:** Retrieve a list of channels that are not assigned to any group.

### Parameters
- **organisation_id** (path): ID of the organisation.

### Responses
- **200**: Successfully fetched ungrouped channels.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Channels not in group fetched successfully",
    "data": [
        {
            "channels_id": "unique_channel_id",
            "name": "Channel-3",
            "description": "Channel-3",
            "organisation_id": "organisation_id",
            "owner_id": "owner_id",
            "users": null,
            "user_count": 0,
            "message_count": 0,
            "archived": false,
            "group_id": null,
            "created_at": "timestamp",
            "deleted_at": null
        }
        // More channel objects...
    ]
}
```

---

## Fetch Discoverable Channels for a User
- **Endpoint:** `/organisations/{organisation_id}/groups/channels/user-no-group`
- **Method:** `GET`
- **Summary:** Retrieves a list of channels that are discoverable for a user who is not assigned to any group in a specific organisation.

### Parameters
- **organisation_id** (path): ID of the organisation.

### Responses
- **200**: List of discoverable channels retrieved successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Discoverable Channels for user fetched successfully",
    "data": []
}
```
- **404**: Organisation not found or invalid request.
```json
{
    "status": "error",
    "status_code": 404,
    "message": "Organisation not found"
}
```

