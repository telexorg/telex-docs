---
sidebar_position: 4
---

# Retrieve All Documents
This endpoint retrieves all documents from a specified collection. Supports optional filtering.

## Endpoint
```http
GET `/agent_db/collections/{collection_name}/documents`
```
### Header
```http
X-AGENT-API-KEY: your-api-key
```

### Retrieving documents - without filter

To retrieve all documents without filtering, the request body should look like this

```json
{
  "filter": {}
}
```

### Retrieving documents - using filters

To retrieve documents using one or more filter, include `field-value` pairs inside the filter object. The response data will be a list containing one or more documents that has exactly the `field` and `value` provided or an empty list if none is found.

**Example:**

```json
{
  "filter": {
    "organisation_id": "org-1234",
    "type": "customer_lead"
  }
}
```

**200 OK Response**
```json
 "status": "success",
  "status_code": 200,
  "message": "Document(s) retrieved successfully",
  "data": [
    {
      "_id": "6816d58826572f013df89a01",
      "type": "customer_lead",
      "lead_data": {
        "name": "Jane Doe",
        "email": "jane.doe@example.com",
        "interest": "Enterprise Plan",
        "status": "new"
      },
      "agent_id": "01965754-f864-7baa-b14e-8d9c50932d1f",
      "organisation_id": "01965748-5ebf-7ba9-a0ff-092ba0a4cc1c",
      "created_at": "2025-05-04T12:00:00Z"
    },
    {
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
  ]
```

**400 Response - Not Found (Applies only to filtering)**
```json
 {
  "status": "error",
  "status_code": 404,
  "message": "No documents found matching the filter",
  "error": "No match found",
  "data": []
}
```
