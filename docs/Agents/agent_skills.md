---
sidebar_position: 3
toc_min_heading_level: 2
toc_max_heading_level: 4
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Agent Skills

Agent Skills are the building blocks of intelligence within Telex AI agents. Each skill represents a specific capability—whether it's answering questions, handling tasks, or integrating with external systems. By combining multiple skills, agents can perform complex operations with precision and context-awareness.


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


#### Sample Structure of an Agent card

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



<!-- The A2A specification protocol defines --- main methods -->