---
sidebar_position: 2
is_draft: true
---

# AI Agents
AI Agents are specialized agents designed for conversational and automated interactions within Telex. They extend the core agent framework with features like `auth_callback` for setup, `bot` for enabling chat capabilities, and `listening_mode` to control message handling. These agents can send and receive messages, participate in threads, and operate in channels or direct messages, making them versatile tools for tasks like monitoring, summarization, and user assistance.

AI Agents are built on the same core architecture as standard agents but have additional specifications and behaviors tailored for conversational use and automated responses. Just like the agents. it can be either an interval, modifier or output agent as discussed in [Telex Agent Overview](/docs/Integrations/intro.md).

## AI Agent Json Spec

Just like standard agents, the agent spec is a JSON object used to register and configure the agents within Telex. For AI agents, it includes all the standard [Agent spec fields](/docs/Integrations/intro.md), and the following additional fields:

```json
{
  ... // initial integration json fields
  "bot": "boolean",
  "listening_mode": ["all", "mentions"],
  "bot_details": {
     "name": "string",
     "avatar_url": "URL",
     "description": "string"
  }
}
```

### Field Breakdown

**bot: boolean**

This field `boolean` enables conversational capabilities for the agent. 

When set to `true`, users can interact with the agent directly in channels or direct messages, as though chatting with a teammate.

---

**listening_mode: ["all", "mentions"]**

This field `array` determines how the agent listens to messages:

- `"all"`: The agent receives all messages in the channel.
- `"mentions"`: The agent only receives messages where it is mentioned using @.

Choose the mode that best fits your agent's purpose — for example, `mentions` mode works best for personal assistants, while `all` is more suitable for monitors or sentiment analyzers.

---

**bot_details**

This provides us with the details for the agent

- `"name"`: The name for the agent
-  `"avatar_url:"`: The logo of the agent
- `"descriptions"`: The description for the agent


## Agent Authentication and Setup Flow
When an AI Agent is installed into an organization on Telex, a two-step registration process ensures both sides — Telex and the Agent, are aware and ready to communicate securely.


### 1️⃣ Auth Callback

Once an AI Agent is added to a Telex organization, Telex initiates an authentication handshake to securely register the agent with that specific organization. As part of this handshake, Telex will send the organization’s unique `org_id` along with an `api_key` — this key is scoped specifically for the organization.

Telex performs this by sending a `POST` request to your agent's `auth_callback` URL:

```
{app_url}/auth_callback
```

Here, `app_url` refers to the base URL of your agent’s server, which you define in your agent spec. The `auth_callback` path is expected to be an endpoint that your server exposes to handle this registration handshake.

In other words, the full `auth_callback` URL is formed by combining your defined `app_url` with `/auth_callback`. Make sure your server exposes an endpoint that matches this URL so it can properly receive the authentication handshake.

So, once your agent is added to any Telex organization, Telex will send a `POST` request to this `auth_callback` endpoint with the following payload:

**Auth Payload:**

```json
{
  "org_id": "<string>",
  "api_key": "<string>"
}
```

- **org_id**: A unique identifier for the organization that has installed your agent.
- **api_key**: A secure, organization-specific token that your agent must use to authenticate all future requests to Telex on behalf of this organization.

> ⚡ **Important:** For each organization that installs your agent, telex will send a unique `api_key`. You must store and associate each `api_key` with its corresponding `org_id`.

---

### 2️⃣ Agent Follow-Up: Confirming Registration

After receiving the `auth_callback` payload from Telex, your agent is expected to complete the registration handshake by making a follow-up `POST` request back to Telex. This follow-up request must include the `api_key` (received from Telex in the `auth_callback` payload) as part of the request headers and sent to the URL below.

**Request URL:**

```
POST https://ping.telex.im/api/v1/agents/callback
```

**Headers**
```
X-TELEX-API-KEY: <api_key>
```

>⚡ **Important**:
The `api_key` must be included in the header exactly as shown above.
Your agent's setup will not be considered complete until this confirmation request has been successfully made.

---

### 🔐 Security Consideration

The `api_key` is your agent’s authentication credential and should be handled like a password:

- Store it securely on your backend.
- Never log it, share it, or expose it in client-side code.
- Each organization will receive a distinct `api_key` — never assume one key applies globally.

This separation ensures that even if one key is compromised, only the affected organization’s access is at risk, not all your installations.

## Sending Messages to Telex
When users interact in Telex — whether in channels or direct messages — messages intended for your AI Agent are routed to the target_url you’ve specified in your integration json spec. When sending a message, Telex sends a structured payload that contains key details about the message, including the `channel_id`, `org_id`, `thread_id` and the `is_dm` flag for direct messages.

#### Incoming Message Payload Example

```json
{
  "message": "message",
  "channel_id": "channel-id",
  "org_id": "org-id",
  "thread_id": "thread-id",
  "is_dm": true // signifies a direct message
  "settings": [
    {
      "label": "setting_label",
      "type": "text",
      "default": "setting_value", // value provided or selected by the user
      "required": true
    }
    ...
  ]
}
```

- `org_id`: Organization of the channel or DM the message is sent from.
- `channel_id`: The ID of the channel or DM the message is sent from.
- `thread_id` The thread ID of the message.
- `message`: The actual message from telex.
- `is_dm`: Set to `true` to indicate the message is a DM.
- `settings`: The list of settings provided in the integration json for the user.

This allows your agent to process the incoming message and craft an appropriate response. Once ready, your agent can send replies back to telex by making a post request to the endpoint below:

**Endpoint**

```
POST https://ping.telex.im/api/v1/return/<channel_id>
```
Note: This request must be authenticated.

### Authorization For Messages

Agents must authenticate with Telex while sending messages by including in their request headers, the API key that was sent to its auth_callback url during setup. It should always be defined like this:

**Header**
```http
X-TELEX-API-KEY: <api_key>
```

As stated before, the API key is provided by Telex via the `auth_callback` endpoint of the agent, and grants the agent scoped access to:

- Send messages to channels, groups, or direct messages.
- Upload files.
- Modify organization-level settings (where permitted).

Be sure to keep this key secure, as it grants your agent privileged access to send messages to telex.

---

### Sending Messages to a Channel

To send a message to a channel, you simple make a `POST` request to the endpoint below, with the channel id attached:

**Endpoint**

```
POST https://ping.telex.im/api/v1/return/<channel_id>
```

**Headers**
```
X-TELEX-API-KEY: <api_key>
```

**Request Body Example**

```json
{
  "channel_id": "0192143b-1b99-7a42-bfa2-7753fae0773f",
  "thread_id": "0195dc7b-938e-2c64-b834-b36ecf90fc09",
  "org_id": "0192143b-1ab8-7a42-85eb-82b2dd86f08e",
  "message": "Hello, how can I help you today?",
  "reply": true, // set when replying a message in a thread
  "username": "TeAgent"
}
```

- `channel_id`: The ID of the target channel.
- `thread_id` The thread ID that came with the original message.
- `org_id`: Organization the channel belongs to.
- `message`: Your actual message or response.
- `reply`: Set to `true` to indicate the message is a reply within a thread.
- `username`: Name of the agent the message will appear from.

---

### Sending Messages in DMs

Telex lets you know whether the message is from a DM by including the `is_dm` flag in the payload it sends to your target URL. The channel_id applies for both channel messages and DMs. Hence, you simple have to respond by attaching the channel_id to the endpoint:

**Endpoint**
```
POST https://ping.telex.im/api/v1/return/<channel_id>
```

**Headers**
```
X-TELEX-API-KEY: <api_key>
```

**Request Body Example (DM)**

```json
{
  "channel_id": "0192143b-1b99-7a42-bfa2-7753fae0773f",
  "org_id": "0192143b-1ab8-7a42-85eb-82b2dd86f08e",
  "thread_id": "7a42bfa2-7753-fae0-773f-82b2dd86f08e",
  "message": "Hey! Need anything?",  
  "reply": false, // set to `true` if replying to the message in a thread
  "username": "TeAgent"
}
```

- `channel_id`: The ID of the target DM.

---

### Replying Messages in a Thread

To reply to a specific message within a thread, your agent must use the thread_id that was sent along with the message. Include this thread_id in the request body and set the `reply` field to `true`. This ensures the message is properly associated with the ongoing thread rather than creating a new one.

**Endpoint**

```
POST https://ping.telex.im/api/v1/return/<channel_id>
```

**Headers**
```
X-TELEX-API-KEY: <api_key>
```

**Request Body Example (Thread Reply)**

```json
{
    "channel_id": "0192143b-1b99-7a42-bfa2-7753fae0773f",
    "thread_id": "0195dc7b-938e-2c64-b834-b36ecf90fc09",
    "org_id": "0192143b-1ab8-7a42-85eb-82b2dd86f08e",
    "message": "Here's the update you requested!",
    "reply": true,
    "username": "TeAgent"
}
```

- `thread_id`: The ID of the thread to which the reply belongs.
- `reply`: Set to `true` to indicate the message is a reply within a thread.
- `channel_id`: The ID of the channel or DM where the thread exists.
- `org_id`: Organization the channel or DM belongs to.
- `message`: The content of your reply.
- `username`: The name of the agent sending the reply.

This allows your agent to participate in threaded conversations, keeping discussions organized and contextual.

---

## Example Use Cases

- AI writing assistants that respond to user prompts in a chat
- Monitoring agents that summarize logs and send alerts
- Meeting summarizers or transcription agents
- Site analysis agents that analyzes websites for broken link and missing meta tags

Ready to build your first agent? Start with the [Integration Creation Guide](/docs/Integrations/intro.md).

