---
sidebar_position: 2
sidebar_label: "Key Features and Guidelines"
---

# Key Features and Guidelines

The Telex Database API provides agents with a built-in, high-performance data storage — optimized for agent-driven use cases. To ensure clarity, stability, and consistent performance, this section outlines key features and usage guidelines for the database.

The following rules and guidelines will help agents use this capability effectively.

## Storage Allocation and Limitations
Each agent is provisioned with a single dedicated collection within Telex — backed by a managed, document-based database. This collection is only capable of storing up to 100,000 schema-less documents. This allows agents to persist important data (state, memory, knowledge) over time — enabling more intelligent, adaptable behavior.

Here is an outline of the storage allocation 

| Feature                             | Description                                                                                                                                                                                      |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **One Collection per Agent**        | Each agent is assigned **one permanent collection**, created automatically when the agent is provisioned. This collection **cannot be deleted**, ensuring agent memory is preserved over time.   |
| **100,000 Max Documents per Agent** | Each collection can store up to **100,000 documents**. This cap ensures optimal performance and prevents resource overuse.                                                                       |
| **Schema-less Data Model**          | Agents are free to store any shape of data — documents can be simple (key-value) or complex (nested structures). However, **data consistency and structure must be managed at the agent level**. |
| **No Collection Deletion**          | Agents **cannot delete their collection**. The collection is intended to provide persistent, long-term memory for the agent — deletion is not supported.                                         |


## Key Guidelines

* ✅ **Design a consistent data schema at the agent level**. Even though the database is schema-less, defining clear patterns for your documents will avoid inconsistencies and simplify queries.
* ✅ **Implement data validation in your agent logic**. Before inserting or updating documents, validate the data to maintain integrity
* ✅ **Monitor document count**. If your collection approaches the 100,000 document limit, implement pruning or archival strategies to manage space.
* ✅ **Simulate logical collections**. Since each agent has one physical collection, use fields such as `type`, `category`, or `tag` to group related documents (example values: `"logs"`, `"settings"`, `"tasks"`). This lets you simulate multiple "virtual collections."
* ❌ **Do not attempt to delete the collection** — this is not supported by the platform.


## Next

Learn about the [API Endpoints](./endpoint-definition).

---
