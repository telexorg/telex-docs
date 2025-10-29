---
sidebar_position: 2
toc_min_heading_level: 2
toc_max_heading_level: 4
title: Create Your First Agent
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Creating Agents

Agents are created through a simple form where you define their identity, role, and behavior. Once launched — with task lists and skills properly configured — you can interact with them to perform tasks based on your defined **task list** using assigned **skills**.

## How to Create an Agent

To create an AI agent, log into your organization and locate the **AI Coworker** section in the sidebar. Click the **Add New** button and fill out the form:

![creating a new agent](/img/create-coworker.png)

## Steps:

### 1. Define the Agent’s Identity
- **Colleague Name**: A friendly name users will recognize
- **Colleague Title**: A short label describing its role (e.g. “Sales Monitor”)

### 2. Write the Job Description
This is the **system prompt** — the agent’s core instruction set.

Example:  
> “You are a resourceful and attentive assistant who helps me stay informed and organized. You proactively surface relevant updates, summarize key information, and communicate clearly and concisely. Always prioritize clarity, usefulness, and speed. If you're unsure about something, ask rather than assume.”

Telex will automatically parse this into a structured system prompt that guides your agent’s behavior.

### 3. Choose a Tone
Select how the agent communicates:
- Friendly  
- Formal  
- Casual  

### 4. Customize the Face (Optional)
Choose a visual avatar that matches your agent’s personality.

### 5. Set Visibility
Control who can use or view the agent:
- **Public**: Available to everyone in the workspace  
- **Private**: Restricted to specific users or Admins  

### 6. Click “Create Colleague”
Once submitted, Telex AI activates the agent which will be operating based on the job description you defined


### Next Steps
- **Task List**: Define what the agent should do  
- **Skills**: Choose capabilities the agent will use to execute tasks
- **JSON Workflow**: Set a worlflow that connects with external systems like N8N tools and custom A2A systems like Mastra