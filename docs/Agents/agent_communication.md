---
sidebar_position: 4
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Request/Response Structure

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