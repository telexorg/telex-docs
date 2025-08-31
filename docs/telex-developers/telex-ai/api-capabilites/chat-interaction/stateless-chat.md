---
sidebar_position: 2
id: chat-stateless
title: Stateless Chat
sidebar\_label: Stateless Chat
---

# Stateless Chat

**Stateless Chat** allows your agent to send a single message to a model and receive a response — without tracking prior conversation history.

This is useful for:

* Answering one-off user queries
* Performing background lookups
* Implementing batch processing workflows
* Content summarization

Because no conversation state is maintained, each request is fully independent.


## 🔗 Endpoint

**POST** `/telexai/chat`


## Required Headers

* `X-AGENT-API-KEY`: Your API key
* Model can be specified via:

  * `X-Model` header
  * `model` query param
  * `model` in request body

See [Chat Overview](./#how-to-specify-a-model) for examples.

## Scenario Examples

### One-off User Question

> A customer support agent needs to look up a product detail in real time:

**Request:**

```json
{
  "messages": [
    {
      "role": "user",
      "content": "What is the return policy for orders over $500?"
    }
  ],
  "stream": false
}
```

**Response:**

```json
{
  "status": "success",
  "status_code": 200,
  "message": "chat completed successfully",
  "data": {
    "Messages": {
      "role": "assistant",
      "content": "For orders over $500, the return period is extended to 45 days. Please ensure the item is in its original packaging."
    }
  }
}
```


### Content Summarization

> An agent is tasked with summarizing a customer review for internal analysis:

**Request:**

```json
{
  "messages": [
    {
      "role": "user",
      "content": "Summarize this review: 'I absolutely love this product! It exceeded my expectations in every way, and the customer service was fantastic. Highly recommend to anyone looking for quality and value.'"
    }
  ],
  "stream": false
}
```

**Response:**

```json
{
  "status": "success",
  "status_code": 200,
  "message": "chat completed successfully",
  "data": {
    "Messages": {
      "role": "assistant",
      "content": "Positive review highlighting product quality, excellent service, and strong recommendation."
    }
  }
}
```

## Usage Notes

* Stateless Chat is best for single-turn use cases
* No context is preserved between calls — send full message in each request
* Use in combination with dynamic model selection ([Model Discovery](../model-discovery))


## Next

* Explore [Contextual Chat](./chat-contextual) for multi-turn conversations
* See [System & Developer Instructions](./chat-instructions) to shape agent behavior

---
