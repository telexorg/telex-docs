---
sidebar_position: 2
---

# Creating Agents

Telex's agent spec supports multiple use-cases, from custom data aggregators, to summarisers, translators and AI agents. You can chose to create an agent specific to your organisation's use, or create an agent that can be used by all Telex users across different organisations.

Agents on Telex are built using Google's Agent2Agent(A2A) Protocol. The A2A Protocol is an open standard designed to facilitate communication and interoperability between independent AI agent systems. In an ecosystem where agents might be built using different frameworks, languages, or by different vendors, A2A provides a common language and interaction model between all of them. You can read about the A2AProtocol in more detail [here](https://google-a2a.github.io/A2A/specification/)

The first step to creating an agent is defining the JSON spec. This JSON spec contains vital information on the name, target url, tick url (for interval type agents), and settings. Telex requests for this JSON, supplied via a URL, to add an agent to an organisation. This means you can define a new Telex agent by hosting a JSON file.
In the A2A protocol, this JSON spec is known as the Agent Card. The structure of the card is shown below:

```json
  {
    name: string;
    description: string;
    url: string;
    provider: AgentProvider;
    version: string;
    documentationUrl?: string;
    capabilities: AgentCapabilities;
    securitySchemes?: { [scheme: string]: SecurityScheme };
    security?: { [scheme: string]: string[] }[];
    defaultInputModes: string[];
    defaultOutputModes: string[];
    skills: AgentSkill[];
    supportsAuthenticatedExtendedCard?: boolean;
  }
```
:::note
Fields suffixed with ? are optional
:::

### Field Definitions

Field Name	Type	Required	Description
id	string	Yes	Unique skill identifier within this agent.
name	string	Yes	Human-readable skill name.
description	string	Yes	Detailed skill description. CommonMark MAY be used.
tags	string[]	Yes	Keywords/categories for discoverability.
examples	string[]	No	Example prompts or use cases demonstrating skill usage.
inputModes	string[]	No	Overrides defaultInputModes for this specific skill. Accepted MIME types.
outputModes	string[]	No	Overrides defaultOutputModes for this specific skill. Produced MIME types.


Sample Structure of an Agent card

```
  {
    "name": "PingPongAgent",
    "description": "An agent that responds 'pong' to 'ping'.",
    "url": "https://telex.im",
    "version": "1.0.0",
    "provider": {
      "organization": "Telex Org.",
      "url": "https://telex.im"
    },
    "documentationUrl": "https://docs.telex.im/docs/Agents"
    "capabilities": {
      "streaming": false,
      "pushNotifications": false,
      "stateTransitionHistory": false
    },
    "defaultInputModes": ["text/plain"];
    "defaultOutputModes": ["application/json", "text/plain"];
    "skills": [
      {
        "id": "erig4w9292tb",
        "name": "Ping Response",
        "description": "Responds with 'pong' when given 'ping'.",
        "inputModes": ["text"],
        "outputModes": ["text"],
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

The agent card lives on a well-known url usually in the form `https://{your_domain}/.well-known/agent.json`. This url is what is used to add your agent to a Telex organization. 

<!-- You can find more details about the agent card [here](https://google-a2a.github.io/A2A/specification/#5-agent-discovery-the-agent-card) -->


## Request/Response Structure
The A2A protocol uses JSON-RPC 2.0 as the payload format for it's requests and responses. JSON-RPC is a remote procedure call protocol encoded in JSON. It allows for calling methods on a remote server and getting responses. It's commonly used for web services and APIs to facilitate communication between a client and a server. Your agent should have two main endpoints:

1. **_GET_** `/.well-known/agent.json` The well-known endpoint which returns the information about the agent on the agent card

2. **_POST_** `/`  
  In JSON-RPC, typically you have just one endpoint (like /) that handles all the method calls. The method you want to invoke is specified in the _method_ field of the JSON-RPC request. The server then looks at this method parameter and routes the request to the appropriate function or handler based on its value. This approach simplifies the API by having a single entry point for all remote procedure calls.

  Sample Request

  ```
  {
    "jsonrpc": "2.0", 
    "method": "subtract", 
    "params": {
      "subtrahend": 23, 
      "minuend": 42
    }, 
    "id": 3
  }
  ```

  Sample Response(Success)
  ```
    {
      "jsonrpc": "2.0", 
      "result": 19, 
      "id": 3
    }
  ```

  Sample Response(Error)

  ```
  {
    "jsonrpc": "2.0", 
    "error": {
      "code": -32600, 
      "message": "Invalid Request"
    }, 
    "id": 3
  }
  ```




## Agent Integration JSON

The Integration JSON content for an agent has some important properties:

1. descriptions `{}` - app_name, app_description, app_url, app_logo
2. [integration_type](/docs/Integrations/intro.md#Interval%20Integrations) - interval, modifier or output
3. integration_category - one of [the available categories](/docs/Integrations/integration-categories.md)
4. [settings](/docs/Integrations/settings.md)`[]`
5. `target_url` and/or `tick_url`.

The integration_type determines if an agent will need a target_url, a tick_url, or both. Agents of type "modifier" only need target_url. This is where they will receive the Telex channel payload. Agents of type "interval" strictly need a tick_url. Telex will call this URL when the required interval is reached. This allows the interval agents to complete its processing and send back data to Telex via the channel webhook when it is ready. Agents of type "output" only need target_url same as modifier agents.

The sections below contain a deeper view of the JSON spec for the three integration types for agents and their caveats.

### Modifier Agent Type

The JSON snippet below outlines a generic representation of a modifier agent. You can copy it as the starting point for your custom agent.

```json
{
  "data": {
    "date": {
      "created_at": "YYYY-MM-DD",
      "updated_at": "YYYY-MM-DD"
    },
    "descriptions": {
      "app_description": "A brief description of the application functionality.",
      "app_logo": "URL to the application logo.",
      "app_name": "Name of the application.",
      "app_url": "URL to the application or service.",
      "background_color": "#HEXCODE"
    },
    "integration_category": "Monitoring & Logging",
    "integration_type": "modifier",
    "is_active": false,
    "output": [
      {
        "label": "output_channel_1",
        "value": true
      },
      {
        "label": "output_channel_2",
        "value": false
      }
    ],
    "key_features": [
      "Feature description 1.",
      "Feature description 2.",
      "Feature description 3.",
      "Feature description 4."
    ],
    "permissions": {
      "monitoring_user": {
        "always_online": true,
        "display_name": "Performance Monitor"
      }
    },
    "settings": [
      {
        "label": "Gender",
        "type": "radio",
        "required": true,
        "default": "Female",
        "options": ["Male", "Female"]
      },
      {
        "label": "Key",
        "type": "text",
        "required": true,
        "default": "1234567890"
      },
      {
        "label": "Do you want to continue",
        "type": "checkbox",
        "required": true,
        "default": "Yes"
      },
      {
        "label": "Provide Speed",
        "type": "number",
        "required": true,
        "default": "1000"
      },
      {
        "label": "Sensitivity Level",
        "type": "dropdown",
        "required": true,
        "default": "Low",
        "options": ["High", "Low"]
      },
      {
        "label": "Alert Admin",
        "type": "multi-checkbox",
        "required": true,
        "default": "Super-Admin",
        "options": ["Super-Admin", "Admin", "Manager", "Developer"]
      }
    ],
    "target_url": "URL for integration or service."
  }
}
```

### Interval Agent Type

The JSON snippet below outlines a generic representation of an interval agent. You can copy it as the starting point for your custom agent.

```json
{
  "data": {
    "date": {
      "created_at": "YYYY-MM-DD",
      "updated_at": "YYYY-MM-DD"
    },
    "descriptions": {
      "app_description": "A brief description of the application functionality.",
      "app_logo": "URL to the application logo.",
      "app_name": "Name of the application.",
      "app_url": "URL to the application or service.",
      "background_color": "#HEXCODE"
    },
    "integration_category": "Monitoring & Logging",
    "integration_type": "interval",
    "is_active": false,
    "output": [
      {
        "label": "output_channel_1",
        "value": true
      },
      {
        "label": "output_channel_2",
        "value": false
      }
    ],
    "key_features": [
      "Feature description 1.",
      "Feature description 2.",
      "Feature description 3.",
      "Feature description 4."
    ],
    "permissions": {
      "monitoring_user": {
        "always_online": true,
        "display_name": "Performance Monitor"
      }
    },
    "settings": [
      {
        "label": "interval",
        "type": "text",
        "required": true,
        "default": "* * * * *"
      },
      {
        "label": "Key",
        "type": "text",
        "required": true,
        "default": "1234567890"
      },
      {
        "label": "Do you want to continue",
        "type": "checkbox",
        "required": true,
        "default": "Yes"
      },
      {
        "label": "Provide Speed",
        "type": "number",
        "required": true,
        "default": "1000"
      },
      {
        "label": "Sensitivity Level",
        "type": "dropdown",
        "required": true,
        "default": "Low",
        "options": ["High", "Low"]
      },
      {
        "label": "Alert Admin",
        "type": "multi-checkbox",
        "required": true,
        "default": "Super-Admin",
        "options": ["Super-Admin", "Admin", "Manager", "Developer"]
      }
    ],
    "tick_url": "URL for subscribing to Telex's clock.",
    "target_url": "Optional URL for getting data from the Telex channel"
  }
}
```

As mentioned earlier, an interval agent will only need the target URL if it needs to collect and store channel messages for further processing. An example of such an agent is the summariser. Whenever it receives a new message for a channel, it stores the message using the channel's channel_id. When the tick_url is called, it summarises the stored messages, constructs the webhook return URL, and sends the summary to Telex. Afterward, it clears its message cache, ready to continue aggregating and summarising new channel messages.

#### Sending Data to Telex

For interval agents, Telex will send a return_url in its /tick_url request so that the agent can send back to the channel after it's done processing.

NB: This is only available to the interval agents.



### Output Agent Type

The JSON snippet below outlines a generic representation of an output agent. You can copy it as the starting point for your custom agent.

```json
{
  "data": {
    "date": {
      "created_at": "YYYY-MM-DD",
      "updated_at": "YYYY-MM-DD"
    },
    "descriptions": {
      "app_description": "A brief description of the application functionality.",
      "app_logo": "URL to the application logo.",
      "app_name": "Name of the application.",
      "app_url": "URL to the application or service.",
      "background_color": "#HEXCODE"
    },
    "integration_category": "Communication & Collaboration",
    "integration_type": "output",
    "is_active": false,
    "output": [
      {
        "label": "output_channel_1",
        "value": true
      },
      {
        "label": "output_channel_2",
        "value": false
      }
    ],
    "key_features": [
      "Feature description 1.",
      "Feature description 2.",
      "Feature description 3.",
      "Feature description 4."
    ],
    "permissions": {
      "monitoring_user": {
        "always_online": true,
        "display_name": "Performance Monitor"
      }
    },
    "settings": [
      {
        "label": "interval",
        "type": "text",
        "required": true,
        "default": "* * * * *"
      },
      {
        "label": "Key",
        "type": "text",
        "required": true,
        "default": "1234567890"
      },
      {
        "label": "Do you want to continue",
        "type": "checkbox",
        "required": true,
        "default": "Yes"
      },
      {
        "label": "Provide Speed",
        "type": "number",
        "required": true,
        "default": "1000"
      },
      {
        "label": "Sensitivity Level",
        "type": "dropdown",
        "required": true,
        "default": "Low",
        "options": ["High", "Low"]
      },
      {
        "label": "Alert Admin",
        "type": "multi-checkbox",
        "required": true,
        "default": "Super-Admin",
        "options": ["Super-Admin", "Admin", "Manager", "Developer"]
      }
    ],
    "tick_url": "URL for subscribing to Telex's clock.",
    "target_url": "Optional URL for getting data from the Telex channel"
  }
}
```
