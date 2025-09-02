---
 sidebar_position: 1
---

# Overview
The Telex Database API is a native, built-in service provided by Telex — giving every agent on the Telex platform its own high-performance, secure data store. It provides a flexible system where agents can save and retrieve information they need to operate.
Instead of having to connect to an external database, agents can use this API to persist information and maintain their own "memory", helping them remember facts, track tasks, and build knowledge over time.

This makes it highly adaptable for a wide range of agent-driven use cases, including:

* Conversational agents with persistent state
* Multi-agent systems requiring shared data
* Personalization engines that adapt to user behavior
* Knowledge management for specialized domains

Unlike traditional databases that require rigid schemas (predefined table structures), the Telex Database API uses documents. These are simple, dynamic records that can store a wide variety of data, from simple text to complex objects. You can think of each document like a digital note or record — agents can create, update, search, and delete these documents as needed.


## What It Supports

The Database API enables:

* Creating collections and documents
* Updating document contents
* Querying documents and collections
* Deleting documents
* Managing document metadata (timestamps, etc)

This supports efficient, secure, and scalable database operations directly from the agent environment — no external database setup required.

## Example Use Cases

**Agent State Management**

Store and retrieve state, preferences, and user profiles to enable personalized conversations.

**Knowledge Base**

Maintain structured knowledge that agents can query — such as FAQs, product catalogs, or domain-specific facts.

**Task Tracking**

Manage lists of active or completed tasks — ideal for workflow automation or multi-step interactions.

**Analytics & Metrics**

Collect and analyze agent interaction data — useful for improving performance, tuning models, and reporting outcomes.

**Multi-Agent Collaboration** 

Provide a shared memory space where multiple agents can read and write — enabling coordination and collective intelligence.


## Why Use Telex Database API?

* **Purpose-built for agents** — no extra database needed
* **Schema-less flexibility** — evolve your data as agent capabilities grow
* **Built-in authentication & security**
* **Fully managed & scalable**


**Next:** Learn about [Key Features and Guidelines](./key-features) 

---
