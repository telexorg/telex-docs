---

sidebar_position: 2
title: Key Features
---

# Key Features

Telex AI provides a flexible and powerful platform for building intelligent, agentic applications on the Telex platform. This section introduces the core capabilities of the API — from accessing state-of-the-art language models to enabling advanced conversational agents.

Whether you're developing chatbots, automating content generation, or building personalized assistants for your agents on Telex, this API gives you the tools to leverage and create rich, AI-powered experiences.

## What It Provides

* **Unified API for Agents**: Agents can access a wide range of models through a consistent interface — no need to manage different providers.
* **Flexible Interaction Modes**: Agents can use stateless chat, multi-turn memory, or system-guided behavior.
* **Model Selection Flexibility**: Agents can specify models via header, query, or body — whichever fits your workflow.
* **Streaming Support** *(coming soon)*: Enable real-time interactions for agents with low-latency responses.

## Core Functionalities

Telex AI currently supports the following core functionalities:

* **Text Generation** — Generate natural, human-like text from prompts. Ideal for chatbots, content creation, summarization, and more.
* **Conversational Agents** — Manage multi-turn dialogue with contextual awareness.
* **Instruction-following** — Perform tasks based on natural language instructions.


*Coming soon*: Vision capabilities, voice interaction, file-based inputs, and advanced multi-modal agents.


## API Endpoints

| Endpoint                 | Purpose                            |
| ------------------------ | ---------------------------------- |
| `/api/v1/telexai/models` | Discover and list available models |
| `/api/v1/telexai/chat`   | Generate conversational responses  |

See the [Endpoint Overview](./endpoint-overview.md) to learn more.

## Example Use Cases

#### Build a Chatbot

Quickly integrate Telex AI into your chatbot applications using the `/chat` endpoint. Maintain conversational context across multiple user turns.

#### Automate Content Generation

Leverage Telex AI to produce marketing copy, summaries, blog posts, or other written content from simple prompts.

#### Personalized AI Assistants

Securely manage agent identities with API keys to build assistants that remember context and provide personalized experiences for each user.

For best practices on managing API keys, see [API Authentication](./authentication).


## 🔗 What’s Next?

Now that you have an overview of what Telex AI can do:

* Explore the [Endpoints](./endpoint-overview) to get started integrating with our AI.

---
