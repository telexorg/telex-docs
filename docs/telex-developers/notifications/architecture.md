---
sidebar_position: 2
---

# Architectural Overview

Telex notifications are delivered through a multi-step architecture involving authentication, token exchange, and real-time subscription to push channels via Centrifugo. This design ensures that Telex clients can receive updates instantly and securely — even when the main Telex app is not running.

## 🔄 High-Level Flow

1. **User Authentication**
   - The client logs in using Telex credentials (`email` + `password`).
   - An access token is returned for authenticated API requests.

2. **Organisation Retrieval**
   - The client fetches the list of organisations the user belongs to.
   - Each organisation corresponds to a unique notification channel.

3. **Notification Token Request**
   - The client requests a **notification token** from the Telex API.
   - This token is required to authenticate with Centrifugo (Telex’s push server).

4. **Subscription Token Request**
   - For each organisation, the client requests a **subscription token** for its notification channel.

5. **WebSocket Connection**
   - The client opens a WebSocket connection to Centrifugo.
   - It sends a `connect` payload containing the notification token and all subscription tokens.

6. **Push Event Handling**
   - Incoming messages are received in JSON format.
   - If the event contains `notification_type == "unread_thread_change"`, the client triggers a native OS notification.


## 🧬 Component Breakdown

| Component               | Role                                                                 |
|------------------------|----------------------------------------------------------------------|
| **Telex API**           | Handles login, organisation lookup, and token issuance               |
| **Centrifugo Server**   | Push server that delivers real-time events via WebSocket             |
| **Telex Client**        | Application that connects to Telex and displays notifications        |
| **WebSocket Layer**     | Maintains persistent connection for push delivery                    |
| **Notification Handler**| Parses incoming events and triggers native alerts                    |


## 🔐 Security Notes

- Tokens are short-lived and stored only in memory (never persisted to disk).
- WebSocket connections use the secure `wss://` protocol.
- Each subscription token is scoped to a specific organisation and channel, ensuring granular access control.
