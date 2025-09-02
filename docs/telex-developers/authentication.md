---
sidebar_position: 2
---

# Authentication

All requests to Telex platform require an authentication key for secure access. This key should be included in the request headers of every API call.

### Where to Get Your API Key

Telex API keys are typically provided:

* Through your Telex developer dashboard or account portal.
* As part of your onboarding credentials.
* Via your organization or team lead (if you're in a managed workspace).

### Usage Format

To authenticate your agent on Telex, provide your issued key in the request header exactly as shown below.

**Header:** 
```http
X-AGENT-API-KEY : your-key
```


### Handling Authentication Key

Upon activating the agent in an organization

* It receives a unique pre-shared key from your platform.
* This key is stored in the server environment or secure vault.
* All outgoing API requests from this agent include the X-AGENT-API-KEY header.

This makes each agent's identity within Telex organizations trackable, revocable, and secure.

:::note
Never share your agent key in public code or frontend applications.
::: 

### Common Authentication Errors

| Status Code | Message             | Meaning                             | Suggested Fix                               |
| ----------- | ------------------- | ----------------------------------- | ------------------------------------------- |
| 401         | invalid API key     | Missing, incorrect, or inactive key | Verify your agent key and header formatting |
| 403         | API key not found   | API key was not provided or was not specified as `X-AGENT-API-KEY` | Verify that the key is provided and specified as `X-AGENT-API-KEY`  |


**Need Help?**

If you're unsure about your agent key or have lost access, reach out to your integration lead or the Telex support team.
