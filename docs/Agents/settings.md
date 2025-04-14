---
sidebar_position: 3
---

# Agent Settings Spec

## Overview

Agents define the type of settings they want users to utilize to input the required data for the agent to function. An agent could define two text fields, and these fields will be used to create a settings entry for the user that activates it. The user can then input values in these text fields on his organization's Apps page. These settings will then be passed to the agent on every call, be it calls to /target_url or /tick_url.

Telex has no business parsing the settings the agent defines except in the case of interval agents, and in this case, Telex only reads the "interval" field defined in the settings. The current version of Telex requires that this field is defined as a text field with expected values set using the crontab syntax. For more information, see the [Interval Integration](/docs/Integrations/intro#interval-integrations) section.

The settings can be configured using JSON format and the following fields:

- Text
- Multi-Select
- Checkbox
- Dropdown
- Number

Each setting type has specific properties that define its behavior and appearance in the agent configuration interface. If a field is kept blank in the definition, Telex will display an empty field for the user to fill in. If a value is set in the definition, Telex will display that value on initial use. The user can then change the value if needed. The value chosen or provided by the user is stored as `default`.

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
  "label": "site-1-url",
  "type": "text",
  "required": true,
  "default": ""
}
```

#### Another Example of a Text Setting

```json
{
  "label": "interval",
  "type": "text",
  "required": true,
  "default": "* * * * *" # crontab syntax for every minute
}
```

---

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
  "default": "Super-Admin,Admin"
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
  "options": ["option1", "option2", "option3"],
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
  "options": ["High", "Low"]
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
