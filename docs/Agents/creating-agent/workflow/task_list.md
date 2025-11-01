---
sidebar_position: 2
---

# Working with Task Lists

The Task List is where you tell your AI agent what you want it to do, in plain language. Think of it as writing a to-do list for your agent, where each task leverages the skills you've assigned to it.

## Understanding Task Lists

A task list is:

- A series of instructions for your agent
- Written in natural language
- Powered by the agent's configured skills
- The foundation for generating a working workflow

## The Task List Interface

When you open the Task List tab, you'll see:

1. **Central Add Button**: A prominent "Add a Task" button in the middle of the screen
2. **Task List Area**: Where your tasks appear after adding them
3. **View Options**: Toggle between Task List, JSON, and Nodes views
4. **Save Button**: To preserve your task list

## Adding Tasks

1. Click the "Add a Task" button
2. Type your task in plain language
3. Click Save to add it to the list

### Example Tasks

```plaintext
"Read all emails from Gmail label 'Reports'"
"Send a daily summary to Slack channel #team-updates"
"Monitor weather in London and alert if rain is predicted"
"Translate incoming messages to Spanish"
"Save customer feedback to our database"
```

### Writing Effective Tasks

Good tasks are:

- Clear and specific
- Action-oriented
- Related to available skills
- Focused on one operation

For example:
✅ "Send daily sales report from Excel to Slack #sales channel"
❌ "Handle sales data" (too vague)

## Building Your Task List

### Step 1: Plan Your Tasks

Before adding tasks, consider:

- What skills your agent has available
- The logical order of operations
- Required inputs and outputs
- Desired outcomes

### Step 2: Add Tasks

1. Click "Add a Task"
2. Write your task clearly
3. Repeat for each required task
4. Arrange tasks in logical order

### Step 3: Review and Refine

- Ensure all tasks are clear and specific
- Check that tasks match available skills
- Verify the task sequence makes sense
- Remove any redundant tasks

## Best Practices

1. **Be Specific**

   - Include relevant details
   - Specify channels, labels, or locations
   - Define expected outputs

2. **Keep it Simple**

   - One action per task
   - Clear, direct language
   - Avoid complex conditions

3. **Consider Dependencies**
   - List tasks in logical order
   - Include necessary input sources
   - Specify output destinations

## Example Task Lists

### Email Processing Workflow

```plaintext
1. "Check Gmail label 'Customer Support' every 30 minutes"
2. "Categorize emails by urgency based on content"
3. "Forward urgent emails to support@company.com"
4. "Post summary to Slack #support channel"
```

### Weather Alert System

```plaintext
1. "Monitor weather forecast for Sydney"
2. "If rain probability > 70%, send alert"
3. "Post daily weather summary to #weather channel"
```

## Tips for Success

1. **Start Small**

   - Begin with a few essential tasks
   - Test the workflow
   - Add more tasks gradually

2. **Use Available Skills**

   - Check your agent's skills first
   - Write tasks that match skill capabilities
   - Request new skills if needed

3. **Be Clear**
   - Use specific details
   - Include necessary parameters
   - Define expected outcomes

After creating your task list:

1. Save your changes
2. Preview the generated workflow
3. Test the workflow
4. Make adjustments as needed

Remember: The task list is designed to be simple and intuitive. You don't need to worry about technical details - just write clear instructions, and the system will handle the rest.

## Next Steps

- Learn about [Agent Workflow JSON](./agent_workflow.md) for advanced configuration
- Explore [Agent Skills](../agent-skills.md) to enhance task capabilities
- Review [Prompts](../prompts.md) for better user interactions
