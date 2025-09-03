---
sidebar_position: 5
---

# Notification Payload Format
When Telex Start receives a push message via the Centrifugo WebSocket, the payload typically follows a consistent structure in JSON format. Understanding this format is key to correctly interpreting and responding to events.


### Sample Payload
  This type of notification signals that a thread has received a new reply or update.
```json
{
  "push": {
    "channel": "<org_id>/<user_id>",
    "pub": {
      "data": {
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
    }
  }
}
```

## Key Fields Explained
- `channel`: Identifies the source of the notification, typically in the format organisation_id/user_id.
- `notification_type`: Describes the nature of the event (e.g., unread_thread_change, new_message, channel_update).
- `section`: Indicates the UI section or module the notification relates to (e.g., channels_section, messages_section).
- `data`: Contains the actual payload relevant to the notification type.
- `notification_id` (optional): A unique identifier for tracking or deduplication.


## Notification Types

Below are the different types of notification events:

| Notification Type | Purpose                                | UI Action Example                          | Backend Action Example                     |
|-------------------|----------------------------------------|--------------------------------------------|--------------------------------------------|
| `new_message`     | A new message was posted               | Show toast or update message list          | Mark thread as active or unread            |
| `channel_update`  | Channel metadata changed               | Refresh channel info in sidebar            | Sync updated channel data to local cache   |
| `mention_alert`   | User was mentioned in a thread         | Highlight thread or send alert notification| Log mention for analytics or reminders     |


Each type would be mapped to a corresponding UI or backend action to ensure a responsive experience.

