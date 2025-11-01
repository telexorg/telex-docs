---
sidebar_position: 3
---

# Agent Workflow JSON Reference

This document provides a comprehensive reference for the workflow JSON structure that powers Telex agents. While you'll primarily interact with workflows through the Task List interface, understanding the JSON structure can be helpful for advanced configurations.

## Complete Example

Below is a complete example of a workflow JSON for a weather assistant agent:

```json
{
  "active": false,
  "category": "utilities",
  "description": "A workflow that gives weather information",
  "id": "sGC3u7y4vBaZww0G",
  "long_description": "\n      You are a helpful weather assistant that provides accurate weather information and can help planning activities based on the weather.\n\n      Your primary function is to help users get weather details for specific locations. When responding:\n      - Always ask for a location if none is provided\n      - If the location name isn't in English, please translate it\n      - If giving a location with multiple parts (e.g. \"New York, NY\"), use the most relevant part (e.g. \"New York\")\n      - Include relevant details like humidity, wind conditions, and precipitation\n      - Keep responses concise but informative\n      - If the user asks for activities and provides the weather forecast, suggest activities based on the weather forecast.\n      - If the user asks for activities, respond in the format they request.\n\n      Use the weatherTool to fetch current weather data.\n",
  "name": "phoenix_agent",
  "nodes": [
    {
      "id": "weather_agent",
      "name": "weather agent",
      "parameters": {},
      "position": [816, -112],
      "type": "a2a/mastra-a2a-node",
      "typeVersion": 1,
      "url": "https://telex-mastra.mastra.cloud/a2a/agent/weatherAgent"
    }
  ],
  "pinData": {},
  "settings": {
    "executionOrder": "v1"
  },
  "short_description": "something"
}
```

## JSON Structure Breakdown

### 1. Basic Information

```json
{
  "active": false,
  "category": "utilities",
  "description": "A workflow that gives weather information",
  "id": "sGC3u7y4vBaZww0G",
  "name": "phoenix_agent",
  "short_description": "something"
}
```

- `active`: Controls whether the workflow is currently running
- `category`: Categorizes the workflow for organization
- `description`: Detailed workflow description
- `id`: Unique identifier (automatically generated)
- `name`: Internal reference name
- `short_description`: Brief summary

### 2. Agent Instructions

```json
{
  "long_description": "\n      You are a helpful weather assistant...\n      Your primary function is to help users get weather details...\n"
}
```

This section contains:

- Agent personality and role definition
- Task-specific instructions
- Response formatting rules
- Required data collection steps
- Tool usage guidelines

### 3. Node Configuration

```json
{
  "nodes": [
    {
      "id": "weather_agent",
      "name": "weather agent",
      "parameters": {},
      "position": [816, -112],
      "type": "a2a/mastra-a2a-node",
      "typeVersion": 1,
      "url": "https://telex-mastra.mastra.cloud/a2a/agent/weatherAgent"
    }
  ]
}
```

Nodes represent individual components in your workflow:

- `id`: Unique node identifier
- `name`: Display name in the interface
- `parameters`: Node-specific configuration
- `position`: Visual coordinates in Task List view
- `type`: Node type identifier
- `typeVersion`: Version for compatibility
- `url`: Associated API endpoint

### 4. Settings and Additional Data

```json
{
  "pinData": {},
  "settings": {
    "executionOrder": "v1"
  }
}
```

Contains workflow-wide configuration:

- `settings`: Global workflow settings
- `pinData`: Persistent data storage
- `executionOrder`: Task execution configuration

## Relationship with Skills

The workflow JSON is tightly integrated with agent skills:

1. **Node Types**: Each node type corresponds to an available skill
2. **Parameters**: Node parameters match skill configuration options
3. **Tool References**: Instructions can reference skills as tools
4. **Validation**: The system ensures tasks only use configured skills

## Best Practices

1. **Task First, JSON Second**

   - Always start with the Task List view
   - Let Telex generate the base JSON
   - Make fine adjustments in JSON if needed

2. **Skill Alignment**

   - Ensure required skills are added before creating tasks
   - Reference skills correctly in node configurations
   - Test skill connections after updates

3. **Version Control**
   - Save workflow versions before major changes
   - Document skill dependencies
   - Track JSON modifications

## Future Capabilities

The workflow system is designed to support:

- Automatic workflow generation from task lists
- Skill-based task suggestions
- Visual workflow builders
- Advanced node configurations
- Custom skill integration

### Converting Tasks to Workflow JSON

When you create tasks in the Task List view, Telex will automatically generates the corresponding workflow JSON. Here's how common tasks map to JSON:

### Example: Gmail to Slack Workflow

```json
{
  "nodes": [
    {
      "id": "gmail_reader",
      "name": "Read Gmail",
      "type": "gmail/read",
      "parameters": {
        "source": "specific_label",
        "label": "Important"
      }
    },
    {
      "id": "slack_sender",
      "name": "Send to Slack",
      "type": "slack/message",
      "parameters": {
        "channel": "#updates",
        "format": "detailed"
      }
    }
  ],
  "settings": {
    "executionOrder": "sequential"
  }
}
```

This JSON is generated from simple task descriptions like:

1. "Read emails from Gmail label 'Important'"
2. "Send results to Slack channel #updates"

## See Also

- [Agent Skills](../agent-skills.md)
- [Task List Guide](./task_list.md)
- [Agent Overview](./agent_workflow.md)
