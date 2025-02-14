---
sidebar_position: 4
---

# Creating Integrations

Telex's integration spec supports multiple use-cases, from custom data aggregators, to summarisers, and translators. You can chose to create an integration specific to your organisation's use, or create an integration that can be used by all Telex users across different organisations. The first step to creating an integration is defining the JSON spec. This JSON spec contains vital information on the name, target url, tick url (for interval type integrations), and settings. In fact, Telex only requests for this JSON, supplied via a URL, to add an integration to an organisation. This means you can define a new Telex integration by hosting a JSON file.

## Integration JSON

The JSON content for an integration has four important properties:

1. descriptions - app_name, app_description, app_url
2. [integration_type](/docs/Integrations/intro.md#Interval%20Integrations)
3. integration_category - one of [the available categories](/docs/Integrations/integration-categories.md)
4. [settings](/docs/Integrations/settings.md)
5. `target_url` and/or `tick_url`

The integration_type determines if an integration will need a target_url, a tick_url, or both. Integrations of type "modifier" only need target_url. This is where they will receive the Telex channel payload. Integrations of type "interval" strictly need a tick_url. Telex will call this URL when the required interval is reached. This allows the interval integration to complete its processing and send back data to Telex via the channel webhook when it is ready. The sections below contain a deeper view of both integration types and their caveats.

### Modifier Integration Type

The JSON snippet below outlines a generic representation of a modifier integration. You can copy it as the starting point for your custom integration.

```json
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
    "integration_category": "Monitoring & Logging",
    "integration_type": "modifier",
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
      "monitoring_user": {
        "always_online": true,
        "display_name": "Performance Monitor"
      }
    },
    "settings": [
      {
        "label": "Gender",
        "type": "radio",
        "required": true,
        "default": "Female",
        "options": ["Male", "Female"]
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
        "options": ["High", "Low"]
      },
      {
        "label": "Alert Admin",
        "type": "multi-checkbox",
        "required": true,
        "default": "Super-Admin",
        "options": ["Super-Admin", "Admin", "Manager", "Developer"]
      }
    ],
    "target_url": "URL for integration or service."
  }
}
```

### Interval Integration Type

The JSON snippet below outlines a generic representation of an interval integration. You can copy it as thr starting point for your custom integration.

```json
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
    "integration_category": "Monitoring & Logging",
    "integration_type": "interval",
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
      "monitoring_user": {
        "always_online": true,
        "display_name": "Performance Monitor"
      }
    },
    "settings": [
      {
        "label": "interval",
        "type": "text",
        "required": true,
        "default": "* * * * *"
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
        "options": ["High", "Low"]
      },
      {
        "label": "Alert Admin",
        "type": "multi-checkbox",
        "required": true,
        "default": "Super-Admin",
        "options": ["Super-Admin", "Admin", "Manager", "Developer"]
      }
    ],
    "tick_url": "URL for subscribing to Telex's clock.",
    "target_url": "Optional URL for getting data from the Telex channel"
  }
}
```

As mentioned earlier, an interval integration will only need the target URL if it needs to collect and store channel messages for further processing. An example of such an integration is the summariser. Whenever it receives a new message for a channel, it stores the message using the channel's channel_id. When the tick_url is called, it summarises the stored messages, constructs the webhook return URL, and sends the summary to Telex. Afterward, it clears its message cache, ready to continue aggregating and summarising new channel messages.

#### Sending Data to Telex

For interval integrations, Telex will send a return_url in its /tick_url request so that the integration can send back to the channel after its done processing. 

NB: This is only available to the interval_inregrations.