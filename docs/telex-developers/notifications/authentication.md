---
sidebar_position: 3
title: Authentication Process
---

# Authentication Process

Telex Start uses a native login dialog to authenticate users before enabling notifications and backend connectivity.

## Login Flow

- When Telex Start launches, it places an icon in the system tray.
- Users can right-click the tray icon and select **“Login”** to open the login dialog.
- The dialog appears at the bottom right corner of the screen and prompts users to input:

  - **Email address**
  - **Password**

- Once logged in, native desktop notifications are enabled.

### Behavior on Success

- A success message dialog box is shown.
- The App retrieves access and notification tokens.
- Connects to Centrifugo and subscribes to notification channels.
- Begins listening for backend updates via WebSocket.
- Notifications begin appearing in real time.

### Behavior on Failure

- An error message is shown.
- The login dialog is re-triggered for retry.
- The app does not proceed until valid credentials are provided.

## UI Components

- **LoginDialog**  
  A modal dialog built with wxWidgets.
  - Includes username and password fields.
  - “Login” and “Cancel” buttons.
  - Validates input before triggering authentication.

- **NotificationWindow**  
  Listens for login events and displays the dialog.
  - Handles login success/failure feedback.
  - Manages token lifecycle and connection state.

---

### Next Steps

- [Installation and Setup](./installation-and-setup)
- [Automatic Subscription Flow](./automatic-subscription-flow)
- [How It Works](./how-it-works.md)