---
sidebar_position: 4
title: Authentication Process
---

# Authentication & Token Management

This section explains how Telex Start authenticates users and manages tokens required to connect to Telex’s backend services and receive notifications.


### Automated Login via `.env`

Telex Start does **not** prompt the user to log in via a GUI. Instead, the user’s **email and password** are defined in a configuration file before the app is launched.

#### File: `env.json`

```json
{
  "email": "user@example.com",
  "password": "securepassword123"
}
```

When the app starts:
- It reads the credentials from env.json
- Sends a login request to the Telex API
- Receives:
  - An access token (for authenticated API requests)
  - A notification token (for push subscriptions)

> These tokens are stored in memory and used throughout the session.


### Token Usage Breakdown

| Token Type         | Purpose                                      | Used In                         |
|--------------------|----------------------------------------------|----------------------------------|
| Access Token       | Authenticates API requests                   | `fetchUserOrganisations()`      |
| Notification Token | Authenticates push subscription requests     | `getSubscriptionToken(channel)` |
| Connection Token   | Enables WebSocket connection via Centrifugo  | `connectAndSubscribeViaWebSocket()` |


### 🔄 Token Flow Summary

- App starts → Reads credentials from env.json
- Login request → Receives access + notification tokens
- Fetch organizations → Uses access token
- Request subscription tokens → Uses notification token
- Connect to WebSocket → Uses connection token


### Implementation Notes

- The login logic is handled in the `Telex` class.
- Tokens are passed as headers in HTTP requests using `cpp-httplib`.
- The connection token is requested via a special API call and used to authenticate with Centrifugo.


### 🛡️ Security Considerations

- Tokens are stored in memory and cleared when the app exits
- Credentials in env.json should be protected and never committed to version control
- Developers should avoid logging tokens or exposing them in UI components
