---
sidebar_position: 5
---

# Notification Payload Structure
When Telex Start receives a push message via the Centrifugo WebSocket, the payload typically follows a consistent structure. Understanding this format is key to correctly interpreting and responding to events.
🔧 General Payload Format

## 🔧 General Payload Format

```json
{
  "push": {
    "channel": "<org_id>/<user_id>",
    "pub": {
      "data": {
        "notification_type": "<type>",
        "section": "<section_name>",
        "data": {
          "channels_id":"01936d11-a66f-7230-a907-cb6048ad051f",
          "name":"General",
          "description":"marktest's general channel",
          "topic":"",
          "organisation_id":"01936d11-a662-7230-ad46-079bbf77e98e",
          "owner_id":"01936d11-79d8-722f-a4eb-6371d395a300",
          "owner_name":"",
          "archived":false,
          "is_private":false,
          "created_at":"2024-11-27T11:01:33.551859+01:00",
          "deleted_at":"0001-01-01T00:00:00Z",
          "access":true,
          "last_thread_id":"0198dd13-464c-7ab6-9939-910b61fadb3c"
        },
        "notification_id": "<optional_id>"
      }
    }
  }
}
```

## Key Fields Explained
- channel: Identifies the source of the notification, typically in the format organisation_id/user_id.
- notification_type: Describes the nature of the event (e.g., unread_thread_change, new_message, channel_update).
- section: Indicates the UI section or module the notification relates to (e.g., channels_section, messages_section).
- data: Contains the actual payload relevant to the notification type.
- notification_id (optional): A unique identifier for tracking or deduplication.


### Example: unread_thread_change
This type of notification signals that a thread has received a new reply or update.

**Sample Payload**
```json
{
  "notification_type": "unread_thread_change",
  "section": "channels_section",
  "data": {
    "channels_id": "01936d11-a66f-7230-a907-cb6048ad051f",
    "name": "General",
    "description": "marktest's general channel",
    "topic": "",
    "organisation_id": "01936d11-a662-7230-ad46-079bbf77e98e",
    "owner_id": "01936d11-79d8-722f-a4eb-6371d395a300",
    "archived": false,
    "is_private": false,
    "created_at": "2024-11-27T11:01:33.551859+01:00",
    "deleted_at": "0001-01-01T00:00:00Z",
    "access": true,
    "last_thread_id": "0198dd13-464c-7ab6-9939-910b61fadb3c"
  }
}
```

### UI Dispatch Logic
Upon receiving this payload, the app triggers a UI event:

```Cpp
wxCommandEvent* event = new wxCommandEvent(wxEVT_SHOW_NOTIFICATION);
event->SetString(notification_data["name"]);
event->SetClientData(new wxStringClientData(notification_data["description"]));
wxQueueEvent(m_pParent, event);
```

## 📡 Handling Other Notification Types

You can extend the logic to handle other types like:

| Notification Type | Purpose                                | UI Action Example                          | Backend Action Example                     |
|-------------------|----------------------------------------|--------------------------------------------|--------------------------------------------|
| `new_message`     | A new message was posted               | Show toast or update message list          | Mark thread as active or unread            |
| `channel_update`  | Channel metadata changed               | Refresh channel info in sidebar            | Sync updated channel data to local cache   |
| `user_typing`     | A user is currently typing             | Show typing indicator in thread view       | Optionally throttle updates for performance|
| `mention_alert`   | User was mentioned in a thread         | Highlight thread or send alert notification| Log mention for analytics or reminders     |


Each type should be mapped to a corresponding UI or backend action to ensure a responsive experience.


### Error Handling & Validation
Before dispatching any event:
- Validate required fields (notification_type, data)
- Log or discard malformed payloads
- Optionally implement retry or fallback logic for critical updates



