---
sidebar_position: 3
---

# Creating AI Agents

Telex empowers you to build versatile AI agents for a wide range of use cases. You can choose to create an agent specific to your organisation's use, or create an agent that can be used by all Telex users across different organisations.

AI agents on Telex are built using Google's Agent2Agent(A2A) Protocol. The A2A Protocol is an open standard designed to facilitate communication and interoperability between independent AI agent systems. In an ecosystem where agents might be built using different frameworks, languages, or by different vendors, A2A provides a common language and interaction model between all of them ensuring that your agent can seamlessly interact with other AI systems. For more information about the A2A Protocol see [Agent2Agent (A2A) Protocol Specification](https://google-a2a.github.io/A2A/specification/)


### The Agent Card

The foundation of any Telex agent is its Agent Card, a JSON specification that defines its core properties and capabilities. This agent card, represented as a JSON object, contains vital information on the name, skills, and capabilities of your agent.  

When adding your agent to Telex, this JSON object is supplied via a publicly accessible URL generally known as the well-known URL. According to the A2A protocol, the recommended location for an agent's Agent Card is `{your_agent_base_url}/.well-known/agent.json`, where `{your_agent_base_url}` is the main web address where your agent is hosted. This well-known URL is where Telex (or any other A2A client) discovers your agent's capabilities and its Agent Card.  

Within this Agent Card, the `url` property specifies the base URL for your agent's A2A service endpoints. This is the address where other agents or services will send actual A2A JSON-RPC requests. The structure of the Agent Card is shown below: 

```
  {
    name: string;
    description: string;
    url: string;
    provider: {
      organization: string;
      url: string;
    };
    version: string;
    documentationUrl?: string;
    capabilities: {
      streaming: boolean;
      pushNotifications: boolean;
      stateTransitionHistory: boolean;
    };
    securitySchemes?: { 
      [scheme: string]: SecurityScheme 
    };
    security?:[{ 
      [scheme: string]: string[] 
      }
    ];
    defaultInputModes: string[];
    defaultOutputModes: string[];
    skills: [
      {
        id: string;
        name: string;
        description: string;
        inputModes: string[];
        outputModes: string[];
        examples: [];
      }
    ];
    supportsAuthenticatedExtendedCard?: boolean;
  }
```
:::note
Fields suffixed with ? are optional
:::

<!-- ### Field Definitions -->


### Sample Structure of an Agent card

```json
  {
    "name": "PingPongAgent",
    "description": "An agent that responds 'pong' to 'ping'.",
    "url": "https://your-agent-domain.com/api",
    "version": "1.0.0",
    "provider": {
      "organization": "Example Org.",
      "url": "https://www.example-organization.com"
    },
    "documentationUrl": "https://your-agent-domain.com/docs",
    "capabilities": {
      "streaming": false,
      "pushNotifications": false,
      "stateTransitionHistory": false
    },
    "defaultInputModes": ["text/plain"],
    "defaultOutputModes": ["application/json", "text/plain"],
    "skills": [
      {
        "id": "erig4w9292tb",
        "name": "Ping Response",
        "description": "Responds with 'pong' when given 'ping'.",
        "inputModes": ["text/plain"],
        "outputModes": ["text/plain"],
        "examples": [
          {
            "input": { "parts": [{ "text": "ping", "contentType": "text/plain" }] },
            "output": { "parts": [{ "text": "pong", "contentType": "text/plain" }] }
          }
        ]
      }
    ],
    "supportsAuthenticatedExtendedCard": false
  }
```

<!-- You can find more details about the agent card [here](https://google-a2a.github.io/A2A/specification/#5-agent-discovery-the-agent-card) -->


## Request/Response Structure
The A2A protocol uses JSON-RPC 2.0 as the payload format for it's requests and responses. JSON-RPC is a remote procedure call protocol encoded in JSON. It allows for calling methods on a remote server and getting responses. It's commonly used for web services and APIs to facilitate communication between a client and a server. Your agent should expose two main endpoints:

1. **_GET_** `/.well-known/agent.json` The well-known endpoint which returns the information about the agent on the agent card

2. **_POST_** `/`  
  JSON-RPC simplifies API design by typically using a single endpoint for all method calls. Your agent's `POST /` endpoint will use the `method` field in the request to route to the appropriate function.

  Sample Request

  ```json
  {
    "jsonrpc": "2.0", 
    "method": "message/send", 
    "id": 3,
    "params": {
      "id": "ping-test-1",
      "message": {
        "role": "user",  
        "parts": [
          {
            "type": "text",
            "text": "ping"
          }
        ]
      }
    } 
  }
  ```

  Sample Response(Success)
  ```json
    {
      "jsonrpc": "2.0", 
      "id": 3,
      "result": {
        "role": "user",  
        "parts": [
          {
            "type": "text",
            "text": "pong"
          },
        ],
        "task_id": "ping-test-1",
        "kind": message
      } 
    }
  ```

  Sample Response(Error)

  ```json
  {
    "jsonrpc": "2.0", 
    "id": 3,
    "error": {
      "code": -32600, 
      "message": "Invalid Request"
    } 
  }
  ```

