---
sidebar_position: 4
sidebar_label: "API Authentication"
---

# Database API Authentication

All requests to the **Database API** must be authenticated using an API key issued to the agent. Agents are typically issued a **pre-shared key** when they are added to an organization — this key is required for all API interactions.


## How to Authenticate

Include the API key in your request header:

**Header**
```text
X-AGENT-API-KEY: your-api-key-here
```

***Example (using curl):***

```bash
curl -X GET https://api.telex.im/api/v1/database/your-endpoint \
  -H "X-AGENT-API-KEY: tlx-agent-abc123xyz"
```


## Common Authentication Errors

| Status Code | Message                     | Meaning                                      | Suggested Fix                         |
| ----------- | --------------------------- | -------------------------------------------- | ------------------------------------- |
| 401         | invalid API key             | Provided key is incorrect or expired         | Verify the API key and header format  |
| 401         | API key not found / missing | No API key provided in the request or the name does not match `X-AGENT-API-KEY`          | Add `X-AGENT-API-KEY` header with key |
| 403         | unauthorized access         | Key is revoked or lacks required permissions | Contact your admin or rotate your key |

## Need Help?

If you are unsure about your API key or have lost access, contact your integration lead or the Telex support team.

---
