---
sidebar_position: 6
is_draft: true
---

# Agent Database

Telex provides a MongoDB database for agents. It is designed to support AI agents in managing and retrieving data efficiently. It provides a structured environment for storing agent-related information, ensuring high performance and reliability. An agent is entitled to one collection only which can not be deleted. The database has a schemaless design and can store a maximum of 100,000 documents for each agent.

## Agent Database API
This details the endpoints and usage for interacting with the Agent Database provided by Telex. Each agent is provisioned with a dedicated MongoDB collection where they can store and manage up to 100,000 schema-less documents.

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

Create a new MongoDB collection for the agent.

**Request Body**:

```json
{
  "collection": "test_collection_2"
}
```

**200 Response**

```json
{
  "status": "success",
  "status_code": 200,
  "message": "Collection created successfully"
}
```

**Note**: Each agent can only create **one** collection. Subsequent attempts will return an error.

---

### Add Document

**POST** `/agent_db/collections/{collection_name}/documents`

Add a new document to the specified MongoDB collection.

**Request Body**:

```json
{
  "document": {
    "title": "Document Title",
    "content": "Document content here.",
    "timestamp": "2025-05-04T12:00:00Z"
  }
}
```
**200 Response**

```json
{
  "status": "success",
  "status_code": 200,
  "message": "Document created successfully"
}
```

**Note**: Documents are schema-less. New fields can be introduced via this endpoint.

---

### Retrieve All Documents

**GET** `/agent_db/collections/{collection_name}/documents`

Retrieve all documents from a specified collection. Supports filtering.

**200 Response**
```json
 "status": "success",
  "status_code": 200,
  "message": "Document(s) retrieved successfully",
  "data": [
    {
      "_id": "6816d58826572f013df89a01",
      "agent_id": "376528-f864-7baa-b14e-8d9c50932d1f",
      "content": "Testing",
      "organisation_id": "88226652-5ebf-7ba9-a0ff-092ba0a4cc1c",
      "title": "Adding documents"
    },
    {
      "_id": "6821941c5d90fd300074e90b",
      "agent_id": "01965754-f864-7baa-b14e-8d9c50932d1f",
      "content": "Checking",
      "organisation_id": "01965748-5ebf-7ba9-a0ff-092ba0a4cc1c",
      "title": "Testing again"
    }
  ]
```

**Optional Request Body for filetering**:
  This will return only the documents where the age is 30

```json
{
  "filter": {
    "age": 30
  }
}
```
**Note**: The agent_id and organisation_id are added to each documents. This means that you can actually filter the documents according to their organisation_id.

---

### Retrieve Document by ID

**GET** `/agent_db/collections/{collection_name}/documents/{document_id}`

Retrieve a single document from a collection using its unique ID.

**200 Response**
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Document retrieved successfully",
  "data": {
      "_id": "6816d58826572f013df89a01",
      "agent_id": "376528-f864-7baa-b14e-8d9c50932d1f",
      "content": "Testing",
      "organisation_id": "88226652-5ebf-7ba9-a0ff-092ba0a4cc1c",
      "title": "Adding documents"
  }   
}
  
```
---

### Update Document

**PUT** `/agent_db/collections/{collection_name}/documents/{document_id}`

Update a document by its ID. You can also introduce new fields when updating.

**Request Body**:

```json
{
  "document": {
    "age": 45,
    "status": "active"
  }
}
```

**200 Response**

```json
{
  "status": "success",
  "status_code": 200,
  "message": "Document updated successfully"
}
```

**Note**: Any field provided in the `document` object will overwrite (existing field) or add to the existing document (new field).

---

### Delete Document

**DELETE** `/agent_db/collections/{collection_name}/documents/{document_id}`

Delete a specific document by ID from the collection.

**200 Response**

```json
{
  "status": "success",
  "status_code": 200,
  "message": "Document deleted successfully"
}
```
---

## Use Cases

1. **Agent State Management**: Store and retrieve the current context for AI agents.
2. **Knowledge Base**: Maintain a repository of information that agents can query.
3. **Task Tracking**: Log and monitor tasks assigned to agents.
4. **Analytics**: Collect and analyze data to improve agent performance.

## Constraints

* **One collection per agent**.
* **Maximum of 100,000 documents per agent**.
* **Schema-less data model**: Data integrity should be ensured by the agent.
* **Collection deletion is not allowed**.

---

For further support, contact the Telex developer team or refer to the agent overview documentation.
