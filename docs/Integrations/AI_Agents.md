---
sidebar_position: 6
is_draft: true
---

# AI Agents
AI Agents are specialized integrations designed for conversational and automated interactions within Telex. They extend the core integration framework with features like `auth_callback` for setup, `bot` for enabling chat capabilities, and `listening_mode` to control message handling. These agents can send and receive messages, participate in threads, and operate in channels or direct messages, making them versatile tools for tasks like monitoring, summarization, and user assistance.

AI Agents are built on the same core architecture as integrations but have additional specifications and behaviors tailored for conversational use and automated responses. Just like integrations. it can be either an interval, modifier or output integration as discussed in [Telex Integration Overview](/docs/Integrations/intro.md).

## Agent Json Spec

Just like integrations, the agent spec is a JSON object used to register and configure the AI agent within Telex. It includes all the standard [integration spec fields](/docs/Integrations/intro.md), and the following additional fields:

```json
{
  "auth_callback": "URL",
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

 **auth_callback: URL**

This is a required field `string` for agents containing the agent's auth_callback url

This is an endpoint on the agent's server that Telex will call via a `POST` request when the agent is activated within an organization. Think of it as a setup hook. This means that when your integration is added to any telex organization, your auth_callback url is called with the following payloads.

**auth payload sent by Telex:**
```json
{
  "org_id": "<string>",
  "api_key": "<string>"
}
```

The `org_id` is the identifier of the organization the agent was added to. The `api_key` is a secret token that the agent can use to interact with Telex (e.g., sending messages, uploading files, modifying org-level settings). This means that you must expose an auth_callback endpoint on your server, where you will receive the org_id and the api key for any organization your integration is added to.

---

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

## Sending Messages to Telex
When users interact in Telex — whether in channels or direct messages — messages intended for your AI Agent are routed to the target_url you’ve specified in your integration.json. Along with these messages, Telex sends a structured payload that contains key details about the message, including the channel_id, org_id, and the is_dm flag for direct messages.

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
- `message`: The actual message.
- `is_dm`: Set to `true` to indicate the message is a DM.
- `settings`: The list of settings provided in the integration json for the user.

This allows your agent to process the incoming message and craft an appropriate response. Once ready, your agent can send replies back to telex by making a post request to the endpoint below:

**Endpoint**

```
POST https://ping.telex.im/api/v1/return/<channel_id>
```
Note: This request must be authenticated.

### Authorization

Agents must authenticate with Telex while sending messages by including in their request headers, the API key that was sent to its auth_callback url. It should be defined like this:

**Header**
```http
X-Telex-API-Key: <api_key>
```

As stated before, the API key is provided by Telex via the `auth_callback` url specified in the Json spec of the agent, and grants the agent scoped access to:

- Send messages to channels, groups, or direct messages.
- Upload files.
- Modify organization-level settings (where permitted).

Be sure to keep this key secure, as it grants your agent privileged access to send messages to telex.

---

### Sending Messages to a Channel

To send a message to a channel, you simple make a post request to the endpoint below, with the channel id attached:

**Endpoint**

```
POST https://ping.telex.im/api/v1/return/<channel_id>
```

**Headers**
```
X-Telex-API-Key: <api_key>
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

Telex lets you know whether the message is from a DM by including the `is_dm` flag in the payload it sends to your target URL. The channel_id applies for both channel messages and DMs. Hence, you simple just have to respond by attaching the channel_id to the endpoint:

**Endpoint**
```
POST https://ping.telex.im/api/v1/return/<channel_id>
```

**Headers**
```
X-Telex-API-Key: <api_key>
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

To reply to a specific message in a thread, your agent needs to include the `thread_id` in the request body and set the `reply` to `true`. This ensures the message is posted as part of the ongoing thread.

**Endpoint**

```
POST https://ping.telex.im/api/v1/return/<channel_id>
```

**Headers**
```
X-Telex-API-Key: <api_key>
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

