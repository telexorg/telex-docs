---
sidebar_position: 3
title: Role Configuration
---

# Role Configuration

Telex uses a role-based access model to help organizations manage permissions, maintain control, and define how users interact with agents and workspace features. This page outlines each role and the actions it enables.

## 👥 Available Roles

### 🛠️ Administrator  
Full access and control across the organization.

Can:
- Remove members from organization  
- Invite members  
- Create custom roles  
- Create channels  
- Comment on threads  
- View billing  
- Create webhooks  
- View channels  
- Change user organization role  


### 👤 Manager  
Read, write, and approve — ideal for team leads.

Can:
- Remove members from organization  
- Invite members  
- Create custom roles  
- Create channels  
- Comment on threads  
- View billing  
- Create webhooks  
- View channels  
- Change user organization role  


### 🧭 Project Lead  
Manage, coordinate, and oversee specific initiatives.

Can:
- Remove members from organization  
- Invite members  
- Create custom roles  
- Create channels  
- Comment on threads  
- View channels  
- Change user organization role  


### ✍️ User  
Standard contributor with read, write, and update access.

Can:
- Invite members  
- Create channels  
- Comment on threads  
- View channels  


### 👀 Guest  
Read-only access for external collaborators or limited roles.

Can:
- View channels only.


## 🔐 Role Impact on Agent Interaction

- **Creating Agents**: Reserved for Administrators and Managers.
- **Using Agents in DMs**: Depends on role and workspace permissions.
- **Mentioning Agents in Channels**: Available to Users, Managers, and Project Leads if the agent is present and permissions allow.

## 🧩 Assigning Roles

Roles are assigned during member onboarding or updated by Administrators. Use roles to reflect team structure and maintain clarity across your workspace.

## 🛡️ Best Practices

- Limit Administrator access to trusted leads.
- Use Manager and Project Lead roles for operational oversight.
- Assign User roles for day-to-day contributors.
- Use Guest roles for external or view-only participants.
- Review roles periodically as your team evolves.

---

### Next Steps

- [Workspace Setup](./workspace_setup.md)
- [Agent Strategy](./agent_strategy.md)
- [Using Agents](../getting_started_with_agents/using_agents.md)