---
sidebar_position: 3
---

# Endpoint Overview

The **Telex Database API** allows agents to store and manage data using a simple set of RESTful endpoints. It is designed to give every agent its own high-performance data store on the Telex platform — eliminating the need for external databases and allowing agents to persist knowledge, state, and context directly within the Telex ecosystem.

This page provides a summary of the available endpoints to help you understand what operations are supported and how to get started.


## Base URL

```text
https://api.telex.im/api/v1
```

All API requests are made to this base URL.

## Authentication

Every request must include your agent's API key:

```text
X-AGENT-API-KEY: your-api-key-here
```

Refer to [Database API Authentication](./database-auth) for setup and usage details.


## API Endpoints

| Operation                                                                  | Method | Endpoint                                                          |
| -------------------------------------------------------------------------- | ------ | ----------------------------------------------------------------- |
| [**Create Collection**](./api-reference/create-collection)             | POST   | `/agent_db/collections`                                           |
| [**Add Document**](./api-reference/create-document)                       | POST   | `/agent_db/collections/{collection_name}/documents`               |
| [**Retrieve All Documents**](./api-reference/retrieve-document)   | GET    | `/agent_db/collections/{collection_name}/documents`               |
| [**Retrieve Document by ID**](./api-reference/retrieve-document-by-id) | GET    | `/agent_db/collections/{collection_name}/documents/{document_id}` |
| [**Update Document**](./api-reference/update-document)                 | PUT    | `/agent_db/collections/{collection_name}/documents/{document_id}` |
| [**Delete Document**](./api-reference/delete-document)                 | DELETE | `/agent_db/collections/{collection_name}/documents/{document_id}` |

These endpoints allow agents to:

* Persist knowledge and long-term memory.
* Track state across user interactions.
* Log tasks, activities, or analytical data.
* Retrieve and update stored information on demand.


## Notes

* Each agent has **one permanent collection** (see [Key Features and Guidelines](./key-features)).
* Collections are flexible and schema-less — agents define their own document structures.


## Next Steps

* Dive into each [API endpoint](./endpoint-definition#api-endpoints) for detailed specs, request bodies, and examples.

