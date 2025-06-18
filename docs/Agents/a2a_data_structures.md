---
sidebar_position: 3
---

# Data objects in A2A protocol

### Message Object  
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

### Part Object  
A part object represents a distinct piece of content within a `Message` or `Artifact`. A Part can be either a **TextPart**, **FilePart**, or **DataPart**, thus it can contain text,  a file or structured data. All Part types also include an optional metadata field for part-specific metadata.

```ts
Part = TextPart | FilePart | DataPart;
```

#### Text Part  
A TextPart object is used to convey plain text content. The kind property contains a string literal "text" used to identify the part as containing textual content while the actual text is stored in the text property. It has an optional metadata field to hold metadata specific to the text part.

```ts
{
  kind: "text";
  text: string;
}
```


### Task Object  
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

#### TaskStatus Object  
This represents the current state and associated context (e.g., a message from the agent) of a Task.

```ts
{
  state: TaskState;
  message?: Message;
  timestamp?: string;
}
```


#### TaskState Enum  
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

### JSON-RPC Structures  
The A2A protocol adheres to the standard JSON-RPC 2.0 structures for requests and responses.  

#### JSONRPCRequest Object  
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

#### JSONRPCResponse Object  
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

#### JSONRPCError Object  
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

