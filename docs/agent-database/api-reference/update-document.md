---
sidebar_position: 5
---

# Update Document
This API endpoint updates a document by its ID. 


## Endpoint

```http
PUT `/agent_db/collections/{collection_name}/documents/{document_id}`
```

### Header
```http
X-AGENT-API-KEY: your-api-key
```

### Request Body 

```json
{
  "document": {}
}
```
  The `document` field is an object where your data contents will go. It is a required field.


#### Sample Request Body 

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

### Sample Responses 

#### 200 - OK   

```json
{
  "status": "success",
  "status_code": 200,
  "message": "Document updated successfully"
}
```

#### 400 - Bad Request 

```json
{
  "status": "error",
  "status_code": 400,
  "message": "no document found with ID: {document_id} in collection {agent_id}_{collection_name}",
  "error": {}
}
```

#### 400 - Bad Request 

```json
{
  "status": "error",
  "status_code": 400,
  "message": "error encountered while validating document fields",
  "error": "field 'fieldname' cannot be an empty string"
}
```

**Note**
- New fields provided during update will be added to the document, and existing fields with matching keys will be overwritten/updated.
- Ensure to enforce data structure and validation and avoid introducing new field where not meaningful or intended.

---