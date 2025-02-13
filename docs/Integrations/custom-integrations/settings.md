---
sidebar_position: 2
---
# Configuring Settings

## Overview
The **Telex Settings** provides configuration options for custom integrations within Telex. This includes various setting types like multi-select, checkbox, dropdown, text, and number inputs that allow users to customize their integration behavior. Below are the available setting types and their configurations. The settings field in your custom integrations JSON file should contain an array of settings objects.

The settings can be configured using JSON format and include:
- Multi-Select Settings
- Checkbox Settings
- Dropdown Settings
- Text Settings
- Number Settings

Each setting type has specific properties that define its behavior and appearance in the integration configuration interface.

### Multi-Select Setting
```json
{
  "label": "Setting Label",
  "type": "multi-select",
  "description": "Description of the multi-select setting.",
  "required": true,
  "default": "option1,option2,option3"
}
```
- **Label**: A descriptive name for the setting.
- **Type**: Indicates that this is a multi-select option.
- **Description**: Provides context or instructions for the user.
- **Required**: Boolean indicating if this setting must be filled.
- **Default**: A comma-separated list of default options.
  
#### Example of a Multi-Select Setting
```json
{
  "label": "Alert Admin",
  "type": "multi-select",
  "description": "Select the user roles to alert.",
  "required": true,
  "default": "Super-Admin,Admin",
}
```
---

### Checkbox Setting
```json
{
  "label": "Setting Label",
  "type": "checkbox",
  "description": "Description of the checkbox setting.",
  "default": false
}
```
- **Label**: A descriptive name for the setting.
- **Type**: Indicates that this is a checkbox option.
- **Description**: Provides context or instructions for the user.
- **Default**: Boolean indicating whether the checkbox is checked or not.
  
#### Example of a Checkbox Setting
```json
{
  "label": "notifyOnError",
  "type": "checkbox",
  "description": "Notify on error",
  "default": false
}
```
---

### Dropdown Setting
```json
{
  "label": "Setting Label",
  "type": "dropdown",
  "options": [
    "option1",
    "option2",
    "option3"
  ],
  "description": "Description of the dropdown setting.",
  "default": "option1",
  "required": true
}
```
- **Label**: A descriptive name for the setting.
- **Type**: Indicates that this is a dropdown option.
- **Options**: An array of possible selections.
- **Description**: Provides context or instructions for the user.
- **Default**: The default selected option.
- **Required**: Boolean indicating if this setting must be filled.
  
#### Example Of a Dropdown setting
```json
{
  "label": "Sensitivity Level",
  "type": "dropdown",
  "required": true,
  "default": "Low",
  "options": [
    "High",
    "Low"
  ]
}
```
---
### Text Setting
```json
{
  "label": "Setting Label",
  "type": "text",
  "description": "Description of the text setting.",
  "default": "default value",
  "required": true
}
```
- **Label**: A descriptive name for the setting.
- **Type**: Indicates that this is a text input.
- **Description**: Provides context or instructions for the user.
- **Default**: The default value for the text input.
- **Required**: Boolean indicating if this setting must be filled.
#### Example of a Text Setting
```json
{
  "label": "interval",
  "type": "text",
  "required": true,
  "default": "* * * * *"
}
```
---

### Number Setting
```json
{
  "label": "Setting Label",
  "type": "number",
  "description": "Description of the number setting.",
  "default": 0,
  "required": true
}
```
- **Label**: A descriptive name for the setting.
- **Type**: Indicates that this is a number input.
- **Description**: Provides context or instructions for the user.
- **Default**: The default numerical value.
- **Required**: Boolean indicating if this setting must be filled.
  
#### Example Of a Number Setting
```json
{
  "label": "maxMessageLength",
  "type": "number",
  "description": "Set the maximum length for incoming messages to format.",
  "default": 500,
  "required": true
}
```
