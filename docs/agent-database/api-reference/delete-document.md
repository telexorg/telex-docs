---
sidebar_position: 6
---

# Delete Document

This API endpoint deletes a specific document from the collection using its ID .

## Endpoint

```http
DELETE `/agent_db/collections/{collection_name}/documents/{document_id}`
```
### Header
```http
X-AGENT-API-KEY: your-api-key
```

## Sample Responses 

### 200 - OK 

```json
{
  "status": "success",
  "status_code": 200,
  "message": "Document deleted successfully"
}
```
### 400 - Not Found 

```json
{
  "status": "error",
  "status_code": 400,
  "message": "no document found with ID: {document_id}",
  "error": {}
}
```

