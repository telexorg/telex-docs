---
sidebar_position: 4
---
# Endpoint Overview

This section provides an overview of the Telex AI API endpoints. These APIs are designed to power **agentic systems integrated with Telex** — enabling your agents to discover models, interact through chat, and build intelligent behaviors within the Telex platform.

## 🔗 Base URL

All endpoints share the following base URL:

```text
https://api.telex.im/api/v1/telexai/
```

## Endpoints 
Telex AI provides two main endpoint at the moment.

| Endpoint  | Method | Description                            |
| --------- | ------ | -------------------------------------- |
| `/models` | GET    | Retrieve available models              |
| `/chat`   | POST   | Send messages to generate AI responses |


## Primary Capabilities
Telex AI provides two primary capabilities through its API — built specifically to support agentic behavior:

### Model Discovery

Use the `/models` endpoint to retrieve a list of models available to your API key. This allows agents to programmatically explore which models they can access and dynamically select models based on their capabilities or user preferences.

For complete details, visit [Model Discovery](./api-capabilites/model-discovery).


### Chat Interaction

The `/chat` endpoint lets agents send user messages to a selected model and receive AI-generated responses. It supports both stateless interactions (single-turn) and contextual conversations (multi-turn), enabling agents to maintain rich, ongoing dialogs.

For complete details, visit [Chat](./api-capabilites/chat-interaction/).


## Additional Notes

* All requests must include your API key: see [Authentication](./authentication).
* Errors are returned as standard HTTP codes. See [Common Authentication Errors](./authentication#-common-authentication-errors).


For detailed usage guides, see:

* [Getting Started](./authentication)
* [Capabilities](./capabilities)
* [Model Discovery](./model-discovery)
* [Chat](./chat-interaction/overview)

Need to troubleshoot? See [Common Authentication Errors](./authentication#-common-authentication-errors).

---
