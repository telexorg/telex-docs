---
sidebar_position: 6
is_draft: true
---

# Agent Database

Telex provides a dedicated, document-based data store for agents. It is designed to support efficient storage and retrieval of agent-related information, allowing agents to store and retrieve structured data in a flexible, schema-less format. This design ensures high performance, reliability, and adaptability for diverse agent use cases.


## Storage Allocation and Limitations

| Feature                          | Description                             |
|----------------------------------|-----------------------------------------|
| One Collection per Agent         | Each agent is assigned **one permanent collection** which **cannot be deleted**. This ensures that all data remains persistently available. |
| Max Documents per Agent          | Each collection can store up to **100,000 documents**. This cap helps maintain optimal performance. |
| Schema-less Data Model           | Agents are free to store any shape of data. However, **data integrity and structure must be managed by the agent itself**. |
| No Collection Deletion Allowed  | Agents are not allowed to delete their collection. Once created, the collection is **undeletable**. |


## Best Practices

- ✅ Design a consistent data schema at the agent level to avoid confusion or malformed data.
- ✅ Implement data validation within your agent logic.
- ✅ Monitor document count regularly if nearing the 100,000-document limit.
- ✅ Use a field like `collectionName` to logically group or categorize data, simulating multiple collections within a single collection (e.g. `"logs"`, `"settings"`, `"tasks"`).
- ❌ Do not attempt to delete collections — it is not supported.

---


## Agent Database API
This details the endpoints for interacting with the Agent Database provided by Telex. As mentioned earlier, each agent is provisioned with a dedicated MongoDB collection where they can store and manage up to 100,000 schema-less documents.

## Base URL

```
https://api.telex.im/api/v1
```

## Authentication

All requests must be authenticated using an API key issued to the agent. Include the API key in the request header as follows:

```
X-TELEX-API-KEY: your-api-key-here
```

## Endpoints

### Create Collection

**POST** `/agent_db/collections`

This endpoint creates a new MongoDB collection for the agent.

**Request Body**:

```json
{
  "collection": "test_collection"
}
```

**200 OK Response**

```json
{
  "status": "success",
  "status_code": 200,
  "message": "Collection created successfully"
}
```

**400 Response**

```json
{
  "status": "error",
  "status_code": 400,
  "message": "Failed to create collection",
  "error": "agent {agent_id} already has a collection"
}
```


**Note**: Each agent can only create **one** collection. Subsequent attempts will return an error as shown above.

---

### Add Document

**POST** `/agent_db/collections/{collection_name}/documents`

This endpoint adds a new document to the specified MongoDB collection.

**Request Body**:

```json
{
  "document": {
    "type": "customer_lead",
    "lead_data": {
      "name": "Jane Doe",
      "email": "jane.doe@example.com",
      "interest": "Enterprise Plan",
      "status": "new"
    },
    "created_at": "2025-05-04T12:00:00Z",
    "updated_at": null
  }
}

```
**200 OK Response**

```json
{
  "status": "success",
  "status_code": 200,
  "message": "Document created successfully"
}
```
**400 Response - Bad Request**
```json
{
  "status": "error",
  "status_code": 400,
  "message": "error encountered while validating document fields",
  "error": "field 'fieldname' cannot be an empty string"
}
```

**Note**
- Documents are schema-less - any valid JSON structure is accepted, and new fields can be added as needed.
- Telex automatically attaches internal fields like agent_id and organisation_id. These should not be manually included.
- While Telex does not enforce a schema, agents are expected to manage structure intentionally and avoid empty or meaningless fields.
- Use null for uninitialized values where appropriate (e.g., "updated_at": null), but avoid empty strings.
- Maintain consistent field naming across documents to prevent fragmentation.

---

### Retrieve All Documents

**GET** `/agent_db/collections/{collection_name}/documents`

This endpoint retrieves all documents from a specified collection. Supports optional filtering.

**Request Body**

To retrieve all documents without filtering.
```json
{
  "filter": {}
}
```
To filter results, include field-value pairs inside the filter object. Example:

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

---

### Retrieve Document by ID

**GET** `/agent_db/collections/{collection_name}/documents/{document_id}`

This endpoint retrieves a single document from a collection using its unique ID.

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

---

### Update Document

**PUT** `/agent_db/collections/{collection_name}/documents/{document_id}`

This endpoint updates a document by its ID. You can also introduce new fields when updating.

**Request Body**:

```json
{
  "document": {
    "lead_data": {
      "name": "Mark Doe",
      "email": "mark.doe@example.com",
      "interest": "Enterprise Plan",
      "status": "old"
    },    
    "created_at": "2025-06-04T12:00:00Z"
  }
}
```

**200 OK Response** 

```json
{
  "status": "success",
  "status_code": 200,
  "message": "Document updated successfully"
}
```

**400 Response - Bad Request**

```json
{
  "status": "error",
  "status_code": 400,
  "message": "no document found with ID: {document_id} in collection {agent_id}_{collection_name}",
  "error": {}
}
```

**400 Response - Bad Request**

```json
{
  "status": "error",
  "status_code": 400,
  "message": "error encountered while validating document fields",
  "error": "field 'fieldname' cannot be an empty string"
}
```

**Note**
- New fields provided will be added to the document, and existing fields with matching keys will be overwritten/updated.
- Ensure to enforce data structure and validation and avoid introducing new field where not meaningful or intended.

---

### Delete Document

**DELETE** `/agent_db/collections/{collection_name}/documents/{document_id}`

This endpoint deletes a specific document by ID from the collection.

**200 OK Response**

```json
{
  "status": "success",
  "status_code": 200,
  "message": "Document deleted successfully"
}
```
**400 Response - Not Found**

```json
{
  "status": "error",
  "status_code": 400,
  "message": "no document found with ID: {document_id}",
  "error": {}
}
```

---

## Use Cases

1. **Agent State Management**: Store and retrieve the current context for AI agents.
2. **Knowledge Base**: Maintain a repository of information that agents can query.
3. **Task Tracking**: Log and monitor tasks assigned to agents.
4. **Analytics**: Collect and analyze data to improve agent performance.


---

For further support, contact the Telex developer team or refer to the agent overview documentation.
