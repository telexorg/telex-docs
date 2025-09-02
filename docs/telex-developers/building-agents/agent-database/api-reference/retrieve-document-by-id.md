---
sidebar_position: 4
---

# Retrieve Document By ID
This endpoint retrieves a single document by its ID from a specified collection.

### Endpoint
```http
GET `/agent_db/collections/{collection_name}/documents`
```
### Header
```http
X-AGENT-API-KEY: your-api-key
```

**200 OK Response**
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Document retrieved successfully",
  "data": {
    "_id": "6821941c5d90fd300074e90b",
    "agent_id": "01965754-f864-7baa-b14e-8d9c50932d1f",
    "organisation_id": "01965748-5ebf-7ba9-a0ff-092ba0a4cc1c",
    "type": "customer_lead",
    "lead_data": {
      "name": "John Doe",
      "email": "john.doe@example.com",
      "interest": "Standard Plan",
      "status": "new"
    },
    "created_at": "2024-05-04T12:00:00Z"
  }   
}
  
```
**404 Response - Not Found**
```json
{
  "status": "error",
  "status_code": 404,
  "message": "Document not found",
  "error": "mongo: no documents in result"
}
```

**400 Response -  Invalid Document ID**
```json
{
  "status": "error",
  "status_code": 400,
  "message": "Failed to retrieve document",
  "error": "invalid ObjectID: the provided hex string is not a valid ObjectID"
}
```

### Notes

> These fields are automatically created for each document. Do not manually them.
- `_id` - This is your unique document ID
- `agent_id` - This is your agent unique ID, 
- `organization_id` - This is the organization ID of the organization the agent is operating on behalf of.