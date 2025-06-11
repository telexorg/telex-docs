---
sidebar_position: 3
toc_min_heading_level: 2
toc_max_heading_level: 4
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Creating Agents

Telex empowers you to build versatile agents, from standard agents to intelligent AI agents, for a wide range of use cases. You can choose to create an agent specific to your organisation's use, or create an agent that can be used by all Telex users across different organisations.

Agents on Telex are built using Google's Agent2Agent(A2A) Protocol. The A2A Protocol is an open standard designed to facilitate communication and interoperability between independent agent systems. In an ecosystem where agents might be built using different frameworks, languages, or by different vendors, A2A provides a common language and interaction model between all of them ensuring that your agent can seamlessly interact with other agentic systems. For more information about the A2A Protocol see [Agent2Agent (A2A) Protocol Specification](https://google-a2a.github.io/A2A/specification/)


### **1. The Agent Card**

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


#### **1.1 Sample Structure of an Agent card**

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


### **2. Data objects in A2A protocol**

### **2.1 Message Object**  
Represents a single piece of contextual information between a client and an agent. Messages are used for instructions, prompts, replies, and status updates. It has the following proprties:

* **_role_**: A string literal representing the Message sender's role. It is **"user"** for messages sent by the client and **"agent"** for messages sent from the server.
* **_parts_**: Usually represented by an array of **Part** objects, holds the content of the Message.
* **_extensions_**: An array of URIs of extensions that are present or contributed to this Message.
* **_referenceTaskIds_**: List of tasks referenced as context by this message.
* **_messageId_**: Identifier created by the message creator
* **_taskId_**: Identifier of task the message is related to
* **_contextId_**: The context the message is associated with
* **_kind_**: A string literla denoting the event type. It is usually "message" for the Message object.
* **_metadata_**: An object that holds any metadata related to the message.

```ts
{
  role: "user" | "agent";
  parts: Part[];
  metadata?: {
    [key: string]: any;
  };
  extensions?: string[];
  referenceTaskIds?: string[];
  messageId: string;
  taskId?: string;
  contextId?: string;
  kind: "message";
}
```

### **2.2 Part Object**  
A part object represents a distinct piece of content within a `Message` or `Artifact`. A Part can be either a **TextPart**, **FilePart**, or **DataPart**, thus it can contain text,  a file or structured data. All Part types also include an optional metadata field for part-specific metadata.

```ts
Part = TextPart | FilePart | DataPart;
```

#### **2.2.1 Text Part**  
A TextPart object is used to convey plain text content. The kind property contains a string literal "text" used to identify the part as containing textual content while the actual text is stored in the text property. It has an optional metadata field to hold metadata specific to the text part.

```ts
{
  kind: "text";
  text: string;
}
```


### **2.3 Task Object**  
A Task represents the stateful unit of work being processed by an A2A Server for an A2A Client. The task object looks like this:

```ts
{
  id: string;
  contextId: string;
  status: TaskStatus;
  history?: Message[];
  artifacts?: Artifact[];
  metadata?: {
    [key: string]: any;
  };
  kind: "task";
}
```

#### **2.3.1 TaskStatus Object**  
This represents the current state and associated context (e.g., a message from the agent) of a Task.

```ts
{
  state: TaskState;
  message?: Message;
  timestamp?: string;
}
```


#### **2.3.2 TaskState Enum**  
Defines the possible lifecycle states of a Task

```ts
{
  Submitted = "submitted",
  Working = "working",
  InputRequired = "input-required",
  Completed = "completed",
  Canceled = "canceled",
  Failed = "failed",
  Rejected = "rejected",
  AuthRequired = "auth-required",
  Unknown = "unknown",
}
```

### **2.4 JSON-RPC Structures**  
The A2A protocol adheres to the standard JSON-RPC 2.0 structures for requests and responses.  

#### **2.4.1 JSONRPCRequest Object**  
All A2A method calls are encapsulated in a **JSON-RPC Request** object. The request object consists of the following properties:

* **_jsonrpc_**: a string literal specifying the version of the JSON-RPC protocol. **MUST** be exactly **"2.0"**.
* **_method_**: A string containing the name of the method to be invoked (e.g., "message/send", "tasks/get").
* **_params_**: A structured value(usually a **Message** object), that holds the parameter values to be used during the invocation of the method. This member may be omitted if the method expects no parameters.
* **_id_**: An identifier established by the Client that MUST contain a String, Integer, or NULL value if included. If it is not included, the request is assumed to be a notification. The value should not be NULL for requests expecting a response, and Integers should not contain fractional parts. The server **MUST** reply with the same id value in the Response object if the id property is included in the request. A2A methods typically expect a response or stream, so **id** will usually be present and non-null.

```ts
{
  jsonrpc: "2.0"; 
  method: String; 
  id: String | Integer;
  params: Message;
}
```

#### **2.4.2 JSONRPCResponse Object**  
Responses from the A2A Server are encapsulated in a JSON-RPC Response object. The response object is made up of the following properties:

* **_jsonrpc_**: A String specifying the version of the JSON-RPC protocol. **MUST** be exactly "2.0".  
* **_id_**: This member is required. It **MUST** be the same as the value of the id member in the JSONRPCRequest Object. If there was an error in detecting the id in the request object (e.g. Parse error/Invalid Request), it's value must be null.
* **EITHER _result_**: This member is required on success. This member **MUST NOT** exist if there was an error invoking the method. The value of this member is determined by the method invoked on the server. It can be a **Task** or **Message** object.
* **OR _error_**: This member is required on failure. This member **MUST NOT** exist if there was no error triggered during invocation. The value of this member MUST be an JSONRPCError object.  

The response object can either have a property of **result** or **error** depending on the status of the operation. The A2A protocol stipulates that these two properties must be mutually exclusive, i.e if the result property is present the error property **must not** be present and if the error property exists, the result property **must not** exist.

```ts
{
  jsonrpc: "2.0"; 
  id: String | Integer | null;
  result?: Task | Message | null;
  error?: JSONRPCError;
}
```

#### **2.4.3 JSONRPCError Object**  
When a JSON-RPC call encounters an error, the Response Object will contain an error member with the followintg properties: 

* code: an integer indicating the error tyoe that occurred
* message: a string providing a shirt descriotion of the error
* data: This value is optional is used to hold additional information about the error. It can be represented using any primitive or structured data type.

```ts
{
  code: Integer;
  message: string;
  data?: any;
}
```

### **3. Request/Response Structure**
The A2A protocol uses JSON-RPC 2.0 as the payload format for its requests and responses. JSON-RPC is a remote procedure call protocol encoded in JSON. It allows for calling methods on a remote server and getting responses. It's commonly used for web services and APIs to facilitate communication between a client and a server. Your agent should expose two main endpoints:

1. **_GET_** `/.well-known/agent.json` The well-known endpoint which returns the information about the agent on the agent card

2. **_POST_** `/`  
  JSON-RPC simplifies API design by using a single endpoint for all method calls. The client requests all A2A RPC methods by sending an HTTP POST request to the A2A Server's base url (as specified in its AgentCard). The body of the HTTP POST request MUST be a JSON-RPC request object, and the Content-Type header MUST be application/json.  Your agent's `POST /` endpoint will use the `method` field in the request to route to the appropriate function.

  **Sample JSON-RPC Request Object**

  ```json
  {
    "jsonrpc": "2.0", 
    "method": "message/send", 
    "id": 3,
    "params": {
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

  **Sample JSON-RPC Success Response**
  ```json
    {
      "jsonrpc": "2.0", 
      "id": 3,
      "result": {
        "role": "agent",  
        "parts": [
          {
            "type": "text",
            "text": "pong"
          }
        ],
        "kind": "message",
        "message_id": "fbuvdhb4ke6vq8hbva9qh9ha"
      } 
    }
  ```

  **Sample JSON-RPC Error Response**

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

**Code Example**:

<Tabs>

  <TabItem value="typescript" label="TypeScript">
    ```ts
    async function handleMessageSend(req: Request, params: any): Promise<any> {
      // Process the incoming message using params
      return
    }

    async function handleTaskSubscription(req: Request, params: any): Promise<any> {
      // Handle subscription to task updates using params
      return
    }


    app.post('/', async (req: Request, res: Response) => {
    const rpcRequest: JsonRpcRequest = req.body;
    const { jsonrpc, method, id, params } = rpcRequest;

    if (jsonrpc !== '2.0' || !method) {
      const errorResponse: JsonRpcErrorResponse = {
        jsonrpc: '2.0',
        id: id === undefined ? null : id,
        error: {
          code: -32600,
          message: 'Invalid Request',
        },
      };
      return res.status(400).json(errorResponse);
    }

    let result: any;
    let errorResponse: JsonRpcErrorResponse | undefined;

    switch (method) {
      case 'message/send':
        result = await handleMessageSend(req, params);
        break;
      case 'task/subscribe':
        result = await handleTaskSubscription(req, params);
        break;
      default:
        // Method not found error
        errorResponse = {
          jsonrpc: '2.0',
          id: id === undefined ? null : id,
          error: {
            code: -32601,
            message: 'Method not found',
          },
        };
        break;
    }

    if (errorResponse) {
      return res.status(405).json(errorResponse);
    }

    // Success response
    const successResponse: JsonRpcSuccessResponse = {
      jsonrpc: '2.0',
      id: id === undefined ? null : id,
      result: result,
    };
    res.json(successResponse);
  });
    ```
  </TabItem>

  <TabItem value="python" label="Python" default>
    ```python
    @app.post("/")
    async def handle_rpc_request(request: Request):
      body = await request.json()

      method = body.get('method')

      if method == "message/send":
        #route to function handling that method
        result = await handle_message_send(request)

      elif method == "task/subscribe":
        result = await handle_task_subscription(request)

      else: 
        #throw return an error if method doesnt exist
        error = {
          "jsonrpc": "2.0", 
          "id": body.get("id", None),
          "error": {
            "code": -32601, 
            "message": "Method not found"
          } 
        }

        return error

      return result

    async def handle_message_send(request):
      #Process the incoming message
      return

    async def handle_task_subscription(request):
      #Handle subscription to task updates
      return
    ```
  </TabItem>
</Tabs>



<!-- The A2A specification protocol defines --- main methods -->