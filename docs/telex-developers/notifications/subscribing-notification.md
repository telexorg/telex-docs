---
sidebar_position: 4
---

# Subscribing to Notifications

Once Telex Start has successfully logged in using credentials from `env.json`, it begins the process of subscribing to real-time notifications. This involves three tightly connected steps:

### Step 1: Retrieving Organizations

After login, the app sends a request to retrieve the organizations the user belongs to.

**Endpoint:**  

`GET /api/v1/users/organisations`

**Headers:**  
```http
Authorization: Bearer <access_token>
```

**Response Example:**
```json
{
  "data": [
    { "id": "org_123", "name": "CyberGroup" },
    { "id": "org_456", "name": "MarkTest" }
  ]
}

```

Each organization ID is used to construct a channel name in the format:

```json
 `<org_id>/<user_id>`
```


These channels are used to subscribe to notifications.

### Step 2: Requesting Subscription Tokens
For each channel, the app requests a subscription token using the notification token.

**Endpoint**:

`POST /api/v1/centrifugo/subscription`

**Headers**:
```http
`X-Notification-Token: <notification_token>`
```
**Body:**

```json
{
  "channel": "org_123/user_789"
}
```


**Response Example:**

```json
{
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

These tokens are stored in a map:
```Cpp
std::map<std::string, std::string> subscriptions;
subscriptions[channel] = subscriptionToken;
```

### Step 3: Connecting to WebSocket
Once all subscription tokens are collected, the app connects to Centrifugo via WebSocket.

**WebSocket URL:**
`wss://api.telex.im/centrifugo/connection/websocket`

**Connection Payload:**
```json
{
  "connect": {
    "token": "<connection_token>",
    "subs": {
      "org_123/user_789": { "token": "<subscription_token>" },
      "org_456/user_789": { "token": "<subscription_token>" }
    }
  }
}
```

> The app sends this payload immediately after the WebSocket Open event.

### Receiving Notifications
Once connected, the app listens for push messages. If a message contains:
```json
{
  "notification_type": "unread_thread_change",
  "data": {
    "name": "General",
    "description": "New reply in launch thread"
  }
}
```

It dispatches a wxEVT_SHOW_NOTIFICATION event to the UI:
```Cpp
wxCommandEvent* event = new wxCommandEvent(wxEVT_SHOW_NOTIFICATION);
event->SetString(notification_data["name"]);
event->SetClientData(new wxStringClientData(notification_data["description"]));
wxQueueEvent(m_pParent, event);
```

### Summary
The subscription flow works as follows:
- Fetch organizations using the access token
- Build channel names and request subscription tokens
- Connect to Centrifugo WebSocket using the connection token
- Subscribe to channels and listen for push notifications

This enables Telex Start to deliver real-time alerts based on backend events.





