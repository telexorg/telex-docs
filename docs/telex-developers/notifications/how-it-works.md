---
sidebar_position: 4
title: How it Works
---

# How Telex Notifications Work

Once Telex Start is installed and the developer logs in, the rest of the notification flow is handled automatically. This section explains the internal architecture — useful for understanding how events are delivered, authenticated, and surfaced via the system tray.

Developers do not need to manually manage tokens, subscriptions, or WebSocket connections. Telex Start handles these behind the scenes.

## End-to-End Flow

Telex delivers real-time notifications through a secure, multi-step architecture involving authentication, token exchange, and WebSocket subscriptions via Centrifugo. This allows Telex clients to receive updates instantly — even when the main Telex app is not running. 

1. **User Login**
   - The user logs in via the Telex Start UI using their email and password.
   - On success, an access token is returned for authenticated API requests.

2. **Organisation Lookup**
   - The client fetches the list of organisations the user belongs to.
   - Each organisation maps to a unique notification channel.

3. **Notification Token Request**
   - The client requests a notification token from the Telex API.
   - This token is used to authenticate with Centrifugo (Telex’s push server).

4. **Subscription Token Request**
   - For each organisation, the client requests a subscription token scoped to that channel.

5. **WebSocket Connection**
   - The client opens a secure WebSocket (`wss://`) connection to Centrifugo.
   - It sends a `connect` payload containing the notification token and all subscription tokens.

6. **Receiving Push Events**
   - Incoming messages are received in JSON format.
   - If the payload includes `notification_type == "unread_thread_change"`, the client triggers a native desktop notification.
   - When Telex Start is running, users can manage notifications and access quick actions via the **system tray icon**.

## System Tray Interaction

The tray icon serves as a lightweight control panel for Telex Start. It allows users to authenticate, launch the main app, test notifications, and shut down the background service — all without opening the full client. 

| Menu Item             | Description                                                  |
|-----------------------|--------------------------------------------------------------|
| **Login**             | Opens the login dialog to authenticate manually              |
| **Open Main App**     | Launches the full Telex desktop client (Flutter-based)       |
| **Close Main App**    | Terminates the full Telex client if it's running             |
| **Test Notifications**| Sends a sample notification to verify UI rendering           |
| **Exit**              | Shuts down Telex Start completely                            |

---
> To open up the menu items, right click the notification icon on the system tray.
> 
> If you don’t see the tray icon, click the “^” arrow near your system clock to expand hidden icons.
---
## Key Components

| Component                | Description                                                           |
|--------------------------|------------------------------------------------------------------------|
| **Telex API**            | Handles login, organisation lookup, and token issuance                 |
| **Centrifugo Server**    | Push server that delivers real-time events via WebSocket               |
| **Telex Start Client**   | Background app that manages login, subscriptions, and notifications    |
| **WebSocket Layer**      | Maintains persistent connection for push delivery                      |
| **Notification Handler** | Parses incoming events and triggers native OS alerts                   |

### Security Highlights

- Tokens are short-lived and stored only in memory (never written to disk).
- All WebSocket traffic uses the secure `wss://` protocol.
- Subscription tokens are scoped per organisation, ensuring granular access control.
- Failed or expired tokens trigger re-authentication via the login dialog.

### Next Steps

- [Installation and Setup](./etup)
- [Authentication Process](./authentication-process.md)
- [Subscribing to Notifications](./subscribing-to-notifications.md)