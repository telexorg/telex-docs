---
sidebar_position: 2
---

# Create Collection
This API endpoint creates a new MongoDB collection for the agent.

## Endpoint

```http
POST `/agent_db/collections`
```

### Header
```http
X-AGENT-API-KEY: your-api-key
```

### Request Body

```json
{
  "collection": "test_collection"
}
```
### Responses

#### 200 - OK 

```json
{
  "status": "success",
  "status_code": 200,
  "message": "Collection created successfully"
}
```

#### 400 - Bad Request

```json
{
  "status": "error",
  "status_code": 400,
  "message": "Failed to create collection",
  "error": "agent {agent_id} already has a collection"
}
```


### Notes 

- Each agent can only create **one** collection. Subsequent attempts will return an error as shown above.
- Deleting of Collections is not supported by the platform for security and auditing purposes.

