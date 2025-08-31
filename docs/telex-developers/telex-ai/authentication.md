---
sidebar_position: 3
id: authentication
title: Authentication
---
#  AI API Authentication

All requests to Telex AI require an API key for secure access. This key should be included in the request headers of every API call.

Visit [Authentication](../agents/authentication) to learn how to get your API key.

### Usage Format

All authenticated requests include this header:

**Header:** 
```http
X-AGENT-API-KEY : your-key
```


**Example (using curl)**

```bash
curl -X GET https://api.telex.im/api/v1/telexai/models \
  -H "X-AGENT-API-KEY: 123xyz"
```


:::note
Never share your agent key in public code or frontend applications.
::: 

### Common Authentication Errors

| Status Code | Message             | Meaning                             | Suggested Fix                               |
| ----------- | ------------------- | ----------------------------------- | ------------------------------------------- |
| 401         | invalid API key     | Missing, incorrect, or inactive key | Verify your agent key and header formatting |
| 403         | API key not found   | API key was not provided or was not specified as `X-AGENT-API-KEY` | Verify that the key is provided and specified as `X-AGENT-API-KEY`  |



## 🔗 What’s Next?

Now that you know how to authenticate your request:

- Learn about the [functionalities](./key-features) Telex AI.
- Explore the full [API Endpoints](./endpoint-overview) to learn how to utilize Telex AI.


**Need Help?**

If you're unsure about your agent key or have lost access, reach out to your integration lead or the Telex support team.
