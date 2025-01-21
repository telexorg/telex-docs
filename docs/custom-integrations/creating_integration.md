---
sidebar_position: 1
---

# Creating Custom Integrations

Telex supports **outbound**, **inbound**, and **bidirectional** custom integrations, enabling seamless communication with third-party systems. Outbound integrations send data from Telex to external services, inbound integrations receive data into Telex, and bidirectional integrations handle both sending and receiving data.

To create a custom integration on Telex, you must set up the integration on your server and provide a **target URL** for Telex to send data to your app or integration.

### Integration Creation Process

During the integration setup, you need to send a JSON configuration object to the Telex server:

Here’s the updated JSON configuration object with the new settings field:

````json
{
    "data": {
        "date": {
            "created_at": "YYYY-MM-DD",
            "updated_at": "YYYY-MM-DD"
        },
        "descriptions": {
            "app_description": "A brief description of the application functionality.",
            "app_logo": "URL to the application logo.",
            "app_name": "Name of the application.",
            "app_url": "URL to the application or service.",
            "background_color": "#HEXCODE"
        },
        "is_active": false,
        "output": [
            {
                "label": "output_channel_1",
                "value": true
            },
            {
                "label": "output_channel_2",
                "value": false
            }
        ],
        "key_features": [
            "Feature description 1.",
            "Feature description 2.",
            "Feature description 3.",
            "Feature description 4."
        ],
        "permissions": {
            "bot_user": {
                "always_online": false,
                "display_name": "Display name of the bot user"
            },
            "events": [
                "Event description 1.",
                "Event description 2.",
                "Event description 3.",
                "Event description 4."
            ]
        },
        "settings": [
            {
                "label": "Gender",
                "type": "radio",
                "required": true,
                "default": "Female",
                "options": [
                    "Male",
                    "Female"
                ]
            },
            {
                "label": "Key",
                "type": "text",
                "required": true,
                "default": "1234567890"
            },
            {
                "label": "Do you want to continue",
                "type": "checkbox",
                "required": true,
                "default": "Yes"
            },
            {
                "label": "Provide Speed",
                "type": "number",
                "required": true,
                "default": "1000"
            },
            {
                "label": "Sensitivity Level",
                "type": "dropdown",
                "required": true,
                "default": "Low",
                "options": [
                    "High",
                    "Low"
                ]
            },
            {
                "label": "Alert Admin",
                "type": "multi-checkbox",
                "required": true,
                "default": "Super-Admin",
                "options": [
                    "Super-Admin",
                    "Admin",
                    "Manager",
                    "Developer"
                ]
            }
        ],
        "slash_commands": [
            {
                "command": "/command_name_1",
                "description": "Description of command 1.",
                "url": "URL associated with command 1."
            },
            {
                "command": "/command_name_2",
                "description": "Description of command 2.",
                "url": "URL associated with command 2."
            },
            {
                "command": "/command_name_3",
                "description": "Description of command 3.",
                "url": "URL associated with command 3."
            }
        ],
        "target_url": "URL for integration or service."
    }
}
````

It is important to note that setting the default field in settings has to be a value in the options array. The settings field is used to configure the integration settings on the Telex dashboard. The target URL is the endpoint where Telex sends data to your integration.


Sending the data as a JSON object to the Telex server for processing. Once the integration is created, activate it by following the guide on [Activate Custom Integrations](/docs/Getting%20-%20Started/activate_custom.md). After activation, the integration can send and receive messages via its specified target URL and settings.

---

### Outbound Communication
Outbound communication refers to data sent from Telex to your integration. The target URL specified in the integration configuration is the destination where Telex sends data. This URL is essential for receiving outbound messages from Telex.

---

### Inbound Communication
Data is received into Telex primarily through webhook URLs. Each channel in Telex has a unique webhook URL. To send data into a specific channel, use the channel’s webhook URL.

#### Obtaining a Webhook URL

1. Navigate to **Channels** on the Telex dashboard.
2. Select the channel where data will be sent.
3. Choose **Webhook Configuration** to view the webhook URL.

#### Sending Data to Telex
There are two supported methods for sending data to Telex:

1. **Using Query Parameters**
   - Data is sent as key-value pairs appended to the webhook URL.
   - This uses a GET request.

   **Example:**
   ```
   https://ping.telex.im/v1/webhooks/c83b4d52fdf5?event_name=test&message=test_webhook_url&status=success&username=collins
   ```

2. **Using JSON in the Request Body**
   - Data is sent as a JSON object in the body of a POST request.

   **Example:**
   ```json
   {
     "channel_id": "c83b4d52fdf5",
     "webhook_slug": "test_webhook_url",
     "action_type": "create",
     "status_code": "200",
     "event_name": "test",
     "username": "collins",
     "retries": 0,
     "status": "success",
     "avatar_url": "https://example.com/avatar.jpg",
     "message": "This is a test message",
     "user_id": "user123",
     "org_id": "org456"
   }
   ```

   Required fields: `channel_id`, `webhook_slug`, `action_type`, `status_code`, `event_name`, `username`, `status`, `message`, `user_id`, and `org_id`. Additional fields are optional.

To receive data into Telex, specify the channels where the data will appear:

- Go to the integration configuration.
- In the output field, select **All Channels** or specific channels.

---

### Bidirectional Communication
Telex supports bidirectional communication for integrations, allowing both sending and receiving of data. Set the output settings of your integration to **true** to enable receiving data from Telex while also sending data to it. This flexibility makes Telex an ideal platform for dynamic, real-time system interactions.


