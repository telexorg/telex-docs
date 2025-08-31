---
sidebar_position: 3
---

# Add Document

This API endpoint adds a new document to the specified MongoDB collection.


## Endpoint

```http
POST `/agent_db/collections/{collection_name}/documents`
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
### Responses

#### 200 - OK  

```json
{
  "status": "success",
  "status_code": 200,
  "message": "Document created successfully"
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
### Important Information

- Documents are schema-less - any valid JSON structure is accepted, and new fields can be added as needed.
- Since only one collection is provided for each agent, it is adviced that agents use some form of a field tag where necessary, to seperate the different form of data they might have. An example might be adding a field that says `tag` or `type`. This would aid the agent to filter and retrieve specific documents.
- Telex automatically creates or attaches internal fields for each document like id, agent_id and organisation_id. These should not be manually included.
- While Telex does not enforce a schema, agents are expected to manage structure intentionally and avoid empty or meaningless fields.
- Use null for uninitialized values where appropriate (e.g., "updated_at": null), but avoid empty strings.
- Maintain consistent field naming across documents to prevent fragmentation.
