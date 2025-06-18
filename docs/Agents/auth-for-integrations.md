---
# sidebar_position: 5
draft: true
---

# Auth for Agents

Agents most times will need to authenticate with a third-party service. The simplest method to make this possible is defining text-fields in the settings that will allow the end user to add the required API keys

## OAuth

If your agent supports OAuth, you must provide an `auth_initiate_url` and `is_oauth`field in your agent's integration JSON. The `auth_initiate_url` represents the URL which Telex will redirect the user to to start the oauth flow when they click on the "Connect App" button. The boolean, `is_oauth` informs Telex if your agent should support the redirection on connect button click. If your agent requires an auth credential which a user can manually input, then you do not need to add this flag. Telex will start the auth process by appending an `api_key` to your `auth_initiate_url`. From there, you can pick it up, store it temporarily, or best, pass it along as state to the upstream oauth service you are working with.

Telex fundamentally does not know that an agent needs OAuth. It only makes the process easy by allowing end users click on a button that initiates the process. The provision of an api_key makes it possible for the agent to write auth credentials to the Telex so that Telex can provide those same credentials at a later time, say when data enters a channel, or an interval is reached.

### OAuth example - Building a Slack Agent

Integrating Slack is a good way to showcase how an agent that works via OAuth should initiate a relationship with Telex. If your custom Slack agent is hosted at https://mycustomslackintegration.com, and your initiate url is https://mycustomslackintegration.com/auth/initiate, Telex will pass the API key to it like this: https://mycustomslackintegration.com/auth/initiate?api_key=tlx_4903039403849393. Now, when you receive a request on the `/auth/initiate` route, you can go ahead to redirect to Slack, passing Telex's api_key as state so as to keep track of the process and call back Telex when it completes. The URL below shows a URL for the Slack oauth page with the requested scopes, redirect URI, and the Telex api key passed along as state:

```
https://slack.com/oauth/v2/authorize?
    client_id=<SLACK_CLIENT_ID>
    &scope=channels:read,chat:write.public
    &redirect_uri=https://mycustomslackintegration.com/redirect
    &state=tlx_4903039403849393
```

When the Slack OAuth process completes, it will redirect to your defined redirect URI with the temporary auth code which is to be exchanged for a permanent one, and the state passed along. This will look like this:

```
https://mycustomslackintegration.com/redirect?
    code=dldkdkddlkdidd9030oidd
    &state=tlx_490303940384939
```

You can then pick up state on this redirect uri and call Telex with the `settings write` endpoint.

## Non Oauth

If your agent requires that your user manually inputs a key, then you simply have to define a setting field for them to achieve this. You can do so with a field of label `text`.
