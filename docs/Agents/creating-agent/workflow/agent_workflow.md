---
sidebar_position: 1
---

# Understanding Agent Workflows

Agent workflows define how your AI coworker processes tasks and interacts with users. A workflow is essentially a list of tasks that your agent needs to perform, powered by the skills you've assigned to it. Think of it as writing a to-do list for your AI assistant, where each task utilizes one or more of its configured skills.

## Prerequisites

Before creating a workflow, ensure you have:

1. [Added necessary skills](../agent-skills.md) to your agent
2. Understood what tasks each skill enables
3. Planned the sequence of tasks you want to automate

## Accessing the Workflow Configuration

1. Navigate to your agent in the **AI Coworkers** section
2. Click on the **Task List** tab
3. Find the **View** button at the top of the page
4. Click to choose between three views:
   - Task List View (visual representation)
   - JSON View (technical configuration)
   - List of Nodes (simplified node list)

## Starting with Tasks

When you first access the Task List tab, you'll see:

1. A clean workspace with an "Add a Task" button at the center
2. Option to import existing workflows
3. View selector (Task List, JSON, or Nodes) in the top right

### Adding Your First Task

1. Click the "Add a Task" button
2. Write your task in plain language, for example:
   - "Read emails from the 'Urgent' Gmail label"
   - "Send daily reports to Slack channel #updates"
   - "Monitor weather for New York and alert if rain is predicted"
3. The system will analyze your task and match it with available skills

### Building the Workflow

As you add tasks, Telex will:

1. Match tasks with appropriate skills
2. Suggest additional related tasks
3. Help configure task parameters
4. Create connections between tasks automatically

## View Options

### Task List View (Default)

- Visual representation of your tasks
- Drag-and-drop task ordering
- Easy task addition and editing
- Clear view of task relationships

### JSON View

When you select "JSON View", you'll see an editor interface with:

- Syntax-highlighted workflow JSON
- Toolbar with actions:
  - Prettify: Formats the JSON for better readability
  - Compress: Minifies the JSON
  - Copy: Copies to clipboard
  - Save: Saves changes

## The Role of Skills

Skills are fundamental to workflows:

1. **Task Enablement**

   - Each task requires one or more skills
   - Skills determine what tasks are possible
   - New skills enable new task types

2. **Task Configuration**

   - Skills provide task parameters
   - Skills determine data formats
   - Skills enable connections between tasks

3. **Examples of Skill-Task Relationships**

| Skill             | Possible Tasks                                                                         |
| ----------------- | -------------------------------------------------------------------------------------- |
| Gmail Integration | - Read emails from specific labels - Monitor for new emails - Send email responses |
| Slack Integration | - Post messages to channels - Monitor channel activity - Send direct messages      |
| Weather API       | - Check weather forecasts - Monitor conditions - Set up weather alerts             |

## Future Capabilities

The workflow system is designed to support:

1. **Automatic Workflow Generation**

   - Write tasks in plain language
   - Click "Generate Workflow"
   - System creates optimized workflow

2. **Smart Task Suggestions**
   - Based on installed skills
   - Learns from common patterns
   - Suggests task optimizations


## Best Practices

1. **Clear Instructions**

   - Write detailed, specific instructions in `long_description`
   - Include all required steps and formats
   - Specify any tools the agent should use

2. **Organized Structure**

   - Group related nodes together
   - Use meaningful names for nodes
   - Keep the workflow focused on a specific purpose

3. **Testing**
   - Always test the workflow after making changes
   - Verify all nodes are properly connected
   - Check that instructions are being followed correctly

## Next Steps

- Learn about [Task Lists](./task_list.md) to understand how to organize agent tasks
- Explore [Agent Skills](../agent-skills.md) to add capabilities to your workflow
- Review [Prompts](../prompts.md) to refine how your agent communicates
