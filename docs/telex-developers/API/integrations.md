# Agents

## Overview
The **Integrations API** provides endpoints to manage integrations for organizations, including Slack integrations, integration creation, retrieval, updates, and deletions. This is organized* into **Integrations**, **Integration Settings**, **Channel Integrations** and **Custom Integrations**.

### Slack OAuth Code to Access Token Exchange
- **Endpoint:** `/slack/organisations/{organisationId}/token-exchg`
- **Method:** `POST`
- **Tags:** Integrations
- **Summary:** Exchange Slack OAuth code for an access token
- **Security:** 
  - `bearerAuth: []`

#### Parameters
- **organisationId** (path): The ID of the organization.
  
#### Request Body
```json
{
  "oauth_code": "your_oauth_code_here"
}
```

#### Responses
- **200**: Slack OAuth token exchange successful.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Slack OAuth token exchange successful",
  "data": {
    "access_token": "your_access_token",
    "incoming_webhook": {
      "channel": "your_channel",
      "channel_id": "your_channel_id",
      "configuration_url": "your_configuration_url",
      "url": "your_webhook_url"
    }
  }
}
```
- **401**: Invalid request body.
- **409**: Integration app already exists.
- **500**: Internal server error.

---

### Get Slack Access Information
- **Endpoint:** `/slack/organisations/{organisationId}`
- **Method:** `GET`
- **Tags:** Integrations
- **Summary:** Get Slack access information
- **Security:** 
  - `bearerAuth: []`

#### Parameters
- **organisationId** (path): The ID of the organization.

#### Responses
- **200**: Slack access info fetched successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Slack access info fetched successfully",
  "data": {
    "id": "your_id",
    "user_id": "your_user_id",
    "organisation_id": "your_organisation_id",
    "access_token": "your_access_token",
    "team_id": "your_team_id",
    "team_name": "your_team_name",
    "channel": "your_channel",
    "channel_id": "your_channel_id",
    "configuration_url": "your_configuration_url",
    "url": "your_url",
    "created_at": "your_created_at",
    "updated_at": "your_updated_at"
  }
}
```
- **401**: Invalid request body.
- **409**: Integration app already exists.
- **500**: Internal server error.

---

### Create a New Integration for the Organization
- **Endpoint:** `/organisations/{organisationId}/integrations`
- **Method:** `POST`
- **Tags:** Integrations
- **Summary:** Create a new integration for the organization

#### Parameters
- **organisationId** (path): The ID of the organization.

#### Request Body
```json
{
  "name": "teleximsss",
  "json_url": "https://www.telex.im",
  "auth_credential": "some-auth-credential",
  "integration_type": "filter"
}
```

#### Responses
- **201**: Integration app created successfully.
```json
{
  "status": "success",
  "status_code": 201,
  "message": "Integration app created successfully",
  "data": {
    "id": "your_integration_id",
    "name": "teleximsss",
    "json_url": "https://www.telex.im",
    "json_schema": {
      "properties": {
        "event": {
          "type": "string",
          "description": "The name of the event"
        },
        "timestamp": {
          "type": "string",
          "description": "The time when the event was triggered"
        }
      },
      "type": "object"
    },
    "auth_credential": "some-auth-credential",
    "integration_type": "filter",
    "is_system_integration": false,
    "created_at": "your_created_at"
  }
}
```
- **400**: Invalid request.
- **404**: Organization not found.

---

### Get Integrations for an Organization
- **Endpoint:** `/organisations/{organisationId}/integrations`
- **Method:** `GET`
- **Tags:** Integrations
- **Summary:** Get integrations for an organization

#### Parameters
- **organisationId** (path): The UUID of the organization.

#### Responses
- **200**: Successfully retrieved the list of integrations.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Integrations retrieved successfully.",
  "data": [
    {
      "id": "your_integration_id",
      "app_name": "Slack",
      "json_url": "https://system-integration.telex.im/slack.json",
      "app_url": "https://slack.com",
      "app_logo": "https://media.tifi.tv/telexbucket/public/logos/slack.png",
      "app_description": "Slack is a cloud-based team business and communication platform.",
      "is_system_integration": true,
      "is_active": false,
      "created_at": "your_created_at",
      "updated_at": "your_updated_at"
    }
    // More integration objects...
  ]
}
```
- **400**: Invalid organisation_id or request.
- **404**: Organisation not found.

---

### Update an Organization's Integration
- **Endpoint:** `/organisations/{organisationId}/integrations/{integrationId}`
- **Method:** `PATCH`
- **Tags:** Integrations
- **Summary:** Update an organization's integration

#### Parameters
- **organisationId** (path): The UUID of the organization.
- **integrationId** (path): The UUID of the integration.

#### Request Body
```json
{
  "json_url": "some-very-weird-logo-url"
}
```

#### Responses
- **200**: Integration updated successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Integrations updated successfully",
  "data": {
    "id": "your_integration_id",
    "name": "instagram",
    "json_url": "some-very-weird-logo-url",
    "auth_credential": "some-auth-credential",
    "integration_type": "",
    "is_system_integration": false,
    "created_at": "your_created_at"
  }
}
```
- **400**: Invalid input.
- **404**: Integration not found.

---

### Delete an Organization's Integration
- **Endpoint:** `/organisations/{organisationId}/integrations/{integrationId}`
- **Method:** `DELETE`
- **Tags:** Integrations
- **Summary:** Delete an organization's integration

#### Parameters
- **organisationId** (path): The UUID of the organization.
- **integrationId** (path): The UUID of the integration.

#### Responses
- **200**: Integration deleted successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Integrations deleted successfully",
  "data": {
    "id": "your_integration_id",
    "name": "instagram",
    "json_url": "some-very-weird-logo-url",
    "auth_credential": "some-auth-credential",
    "integration_type": "",
    "is_system_integration": false,
    "created_at": "your_created_at"
  }
}
```
- **400**: Invalid input.
- **404**: Integration not found.

---

# Integration Settings API Documentation

## Overview
The **Integration Settings API** provides endpoints to manage settings for integrations within organizations.

### Add a New Integration Setting
- **Endpoint:** `/organisations/{organisation_id}/integrations/{integration_id}/settings`
- **Method:** `POST`
- **Tags:** IntegrationSettings
- **Summary:** Add a new integration setting

#### Parameters
- **organisation_id** (path): The UUID of the organization.
- **integration_id** (path): The UUID of the integration.

#### Request Body
```json
{
  "form_field_value": "shallom",
  "form_field_label": "micah bawa"
}
```

#### Responses
- **200**: Successfully added the integration setting.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Integration setting added successfully"
}
```
- **400**: Invalid request.

---

### Retrieve Integration Settings
- **Endpoint:** `/organisations/{organisation_id}/integrations/{integration_id}/settings`
- **Method:** `GET`
- **Tags:** IntegrationSettings
- **Summary:** Retrieve integration settings

#### Parameters
- **organisation_id** (path): The UUID of the organization.
- **integration_id** (path): The UUID of the integration.

#### Responses
- **200**: Successfully retrieved the integration settings.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Integration setting retrieved successfully",
  "data": [
    {
      "id": "your_setting_id",
      "org_id": "your_org_id",
      "integration_id": "your_integration_id",
      "form_field_value": "This is ",
      "form_field_label": "Shallom Micah Bawa",
      "created_at": "your_created_at",
      "updated_at": "your_updated_at"
    }
    // More setting objects...
  ]
}
```
- **404**: Organisation or integration not found.

---

### Update an Existing Integration Setting
- **Endpoint:** `/organisations/{organisation_id}/integrations/{integration_id}/settings`
- **Method:** `PATCH`
- **Tags:** IntegrationSettings
- **Summary:** Update an existing integration setting

#### Parameters
- **organisation_id** (path): The UUID of the organization.
- **integration_id** (path): The UUID of the integration.

#### Request Body
```json
{
  "form_field_label": "Shallom Micah Bawa"
}
```

#### Responses
- **200**: Successfully updated the integration setting.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Integration setting updated successfully"
}
```
- **404**: Organisation or integration not found.
- **400**: Invalid request.

---

## Channel Integrations Settings

The **Channel Integrations API** provides endpoints to manage integrations specific to channels within organizations.

### Add a New Channel Integration Setting
- **Endpoint:** `/organisations/{org_id}/integrations/{integration_id}/channels/{channel_id}/settings`
- **Method:** `POST`
- **Tags:** Channel Integration Settings
- **Summary:** Add a new setting for a channel integration

#### Parameters
- **org_id** (path): The ID of the organization.
- **integration_id** (path): The ID of the integration.
- **channel_id** (path): The ID of the channel.

#### Request Body
```json
{
  "form_field_label": "Shallom Micah",
  "form_field_value": "Bawa"
}
```

#### Responses
- **200**: Channel Integration setting added successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Channel Integration setting added successfully"
}
```

---

### Retrieve Channel Integration Settings
- **Endpoint:** `/organisations/{org_id}/integrations/{integration_id}/channels/{channel_id}/settings`
- **Method:** `GET`
- **Tags:** Channel Integration Settings
- **Summary:** Retrieve all settings for a specific channel integration

#### Parameters
- **org_id** (path): The ID of the organization.
- **integration_id** (path): The ID of the integration.
- **channel_id** (path): The ID of the channel.

#### Responses
- **200**: Channel Integration settings retrieved successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Channel Integration settings retrieved successfully",
  "data": [
    {
      "id": "your_setting_id",
      "org_id": "your_org_id",
      "integration_id": "your_integration_id",
      "channel_id": "your_channel_id",
      "form_field_value": "you",
      "form_field_label": "I love",
      "created_at": "your_created_at",
      "updated_at": "your_updated_at"
    }
    // More setting objects...
  ]
}
```

---

### Update a Channel Integration Setting
- **Endpoint:** `/organisations/{org_id}/integrations/{integration_id}/channels/{channel_id}/settings/{setting_id}`
- **Method:** `PATCH`
- **Tags:** Channel Integration Settings
- **Summary:** Update a specific setting for a channel integration

#### Parameters
- **org_id** (path): The ID of the organization.
- **integration_id** (path): The ID of the integration.
- **channel_id** (path): The ID of the channel.
- **setting_id** (path): The ID of the setting.

#### Request Body
```json
{
  "form_field_value": "ouuuuuuu"
}
```

#### Responses
- **200**: Integration setting updated successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Integration setting updated successfully"
}
```

---

### Delete a Channel Integration Setting
- **Endpoint:** `/organisations/{org_id}/integrations/{integration_id}/channels/{channel_id}/settings/{setting_id}`
- **Method:** `DELETE`
- **Tags:** Channel Integration Settings
- **Summary:** Delete a specific setting for a channel integration

#### Parameters
- **org_id** (path): The ID of the organization.
- **integration_id** (path): The ID of the integration.
- **channel_id** (path): The ID of the channel.
- **setting_id** (path): The ID of the setting.

#### Responses
- **200**: Channel Integration setting deleted successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Channel Integration setting deleted successfully"
}
```


---

## Custom Integrations API Documentation

The **Custom Integrations API** provides endpoints to manage custom integrations for organizations, including creation, retrieval, updating, and deletion of integrations and their settings.

### Create a New Integration for the Organization
- **Endpoint:** `/organisations/{organisationId}/integrations/custom`
- **Method:** `POST`
- **Tags:** Custom Integrations
- **Summary:** Creates a new integration within the specified organization.

#### Parameters
- **organisationId** (path): The ID of the organization.

#### Request Body
```json
{
  "json_url": "https://www.bananaisland.com/banana.json"
}
```

#### Responses
- **201**: Integration app created successfully.
```json
{
  "status": "success",
  "status_code": 201,
  "message": "Integration app created successfully"
}
```
- **400**: Invalid JSON URL supplied.

---

### Get Integrations for an Organization
- **Endpoint:** `/organisations/{organisationId}/integrations/custom`
- **Method:** `GET`
- **Tags:** Custom Integrations
- **Summary:** Retrieve a list of integrations for a specific organization.

#### Parameters
- **organisationId** (path): The UUID of the organization.

#### Responses
- **200**: Successfully retrieved the list of integrations.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Integrations retrieved successfully.",
  "data": [
    {
      "id": "0192202e-b6f3-7d23-8025-3f60675cc031",
      "app_name": "Slack",
      "json_url": "https://system-integration.telex.im/slack.json",
      "app_url": "https://slack.com",
      "app_logo": "https://media.tifi.tv/telexbucket/public/logos/slack.png",
      "app_description": "Slack is a cloud-based team business and communication platform.",
      "is_active": false,
      "created_at": "2024-09-23T19:59:46.166781+01:00",
      "updated_at": "2024-09-23T20:00:08.086829+01:00"
    }
    // More integration objects...
  ]
}
```
- **400**: Invalid organisation_id or request.
- **404**: Organisation not found.

---

### Update an Organization's Integration
- **Endpoint:** `/organisations/{organisationId}/integrations/custom/{integrationId}`
- **Method:** `PUT`
- **Tags:** Custom Integrations
- **Summary:** Update an organization's integration.

#### Parameters
- **organisationId** (path): The ID of the organization.
- **integrationId** (path): The ID of the integration.

#### Request Body
```json
{
  "json_url": "https://www.bananaisland.com/banana.json"
}
```

#### Responses
- **200**: Integration updated successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Integrations updated successfully"
}
```
- **400**: Invalid input.
- **401**: Unauthorized.
- **404**: Integration not found.

---

### Delete an Organization's Integration
- **Endpoint:** `/organisations/{organisationId}/integrations/custom/{integrationId}`
- **Method:** `DELETE`
- **Tags:** Custom Integrations
- **Summary:** Delete an organization's integration.

#### Parameters
- **organisationId** (path): The ID of the organization.
- **integrationId** (path): The ID of the integration.

#### Responses
- **200**: Integration deleted successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Integrations deleted successfully"
}
```
- **400**: Invalid input.
- **401**: Unauthorized.
- **404**: Integration not found.

---

### Update an Organization's Custom Integration Settings
- **Endpoint:** `/organisations/{organisationId}/integrations/custom/{integrationId}/settings`
- **Method:** `PUT`
- **Tags:** Custom Integrations
- **Summary:** Update an organization's custom integration settings.

#### Parameters
- **organisationId** (path): The ID of the organization.
- **integrationId** (path): The ID of the integration.

#### Request Body
```json
{
  "setting_entry": {
    "field_name": "your_field_name",
    "value": "your_value"
  }
}
```

#### Responses
- **200**: Custom integration settings updated successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Integrations Settings updated successfully"
}
```
- **400**: Invalid input.
- **401**: Unauthorized.
- **404**: Integration not found.

---

### Get Integration Settings for an Organization
- **Endpoint:** `/organisations/{organisationId}/integrations/custom/{integrationId}/settings`
- **Method:** `GET`
- **Tags:** Custom Integrations
- **Summary:** Retrieve a list of integration settings for a specific organization.

#### Parameters
- **organisationId** (path): The ID of the organization.
- **integrationId** (path): The ID of the integration.

#### Responses
- **200**: Successfully retrieved the list of integration settings.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Integrations retrieved successfully.",
  "data": {
    "field_name": "your_field_name",
    "value": "your_value"
  }
}
```
- **400**: Invalid organisation_id or request.
- **404**: Organisation not found.
