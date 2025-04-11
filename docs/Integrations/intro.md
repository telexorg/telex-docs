---
sidebar_position: 1
---

# Telex Integrations Overview

Telex is an integrations marketplace. The platform is built to allow developers to write scripts or build simple executables that collect data from a source and post it to a [Telex channel](/docs/Channels/intro). An example integration is a Summarizer that collects data from a Telex channel over a set period of time and posts a summary of all messages in that period back to the channel. Another example is a Bitcoin price tracker that uses the coingecko API to get the current price of Bitcoin and posts it every 30 minutes. A third example is a profanity filter that masks profane words in every new message entering a channel. On analyzing the examples above, it is clear that there are different categories of integrations. Broadly speaking, there are three categories:

1. Modifier integrations: These modify new messages entering a channel and are not time bound. The must return a response within 1 second of receiving a message. Because their response is sent back to the channel immediately.
2. Interval integrations: These send messages to a channel at their set intervals. They may or may not receive new messages entering the channel.
3. Output integrations: Similar to modifier integrations, but get called whenever data enters a channel. They are like routers, sending new data coming into the Telex channel to external sources like discord, Google Sheet, even email.

Below is a better explanation of the three categories.

## Modifier Integrations

Modifier integrations are integrations that modify new messages entering a channel. They are not time bound and do not send messages to the channel at set intervals. Examples of modifier integrations include: Profanity filter, capitalizer, text translator, etc. A hard limit of 1 second is set for modifier integrations, so this means they must return a response within 1 second of receiving a message, else their contribution is skipped. Modifier integrations do not have the concept of a return_url neither do they have a tick_url. They only have a target_url endpoint where they receive new messages entering the channel. They receive messages in this format (at the /target_url):

```json
{
  "message": "message",
  "channel_id": "channel-id",
  "org_id": "org-id",
  "thread_id": "thread-id",
  "settings": [
    {
      "label": "setting_label",
      "type": "text",
      "default": "setting_value",
      "required": true
    }
    ...
  ]
}
```

The format above let's them know what to do with the message. For example, a profanity filter integration will receive a message and the settings will contain the list of words to mask. The integration responds back when it is called with the modified message in the format below:

```json
{
  "message": "modified_message"
}
```

Any deviation from the format above will result in the integration being skipped or its contribution not being added to the channel.

### Integration Chaining

Modifier integrations can be chained together to create more complex integrations. For example, a message can be passed through a grammar corrector, profanity filter, and finally a text translator. The order of the integrations in the chain is important as the output of one integration is the input of the next integration in the chain. The order can be set from the channel's app configuration page.

## Interval Integrations

Interval intergrations are integrations that send messages to a channel at set intervals. They may or may not receive new messages entering the channel. Examples of interval integrations include: Bitcoin price tracker, Stripe Payment monitor, Server status monitor, website uptime monitor, etc. For interval integrations, the clock time is maintained by Telex and the integrations are expected to send data to the channel when Telex requests it. Below are two examples of interval integrations. Interval integrations must define two endpoints in their JSON:

1. A `target_url` endpoint where channel data can be sent to
2. A `tick_url` endpoint where Telex can request data from the integration when its defined interval is reached

### Website Uptime Monitor

For the case of a website uptime monitor for example, if the websites set are https://google.com and https://facebook.com, and the intervals for checking are set to 5 minutes, it means Telex will request a response every 5 minute, and when it does, the uptime monitor is meant to check the status of these websites and return the result to the channel using the channel webhook URL.

![Website uptime monitor archtiecture](/img/integrations/website-monitor-architecture.png)
_Website Uptime monitor integration architecture_

### Channel Summarizer

The summarizer integration is an interval integration and such, it's timing will be maintained by Telex. Every new message that enters a channel where it is activated will be sent to the integration, just like in the case of a modifier integration. When the defined interval is reached, Telex will call its `tick_url` endpoint and the integration is expected to summarize all the messages it has received in the last interval and post the summary to the channel using the channel webhook URL.

## Output Integrations

These integrations can be thought of as routers. Whenever new data enters a Telex channel that they are activated in, they route that data somewhere. Telex has no knowledge of where these integrations send their data to. All Telex does is call the integration's /target_url when new data is added to the channel. There could be a case where multiple integrations exist for a channel.

Here's a scenario where an output integration is useful:

- You have a channel where you want to monitor the uptime of a website
- You need to inform the DevOps team whenever that website is down
- The DevOps team cannot be looking at the channel data every second because they are busy
- You build an email **output** integration that send emails to the addresses specified in the settings
- You activate this integration on your channel
- Telex sends data about website downtimes to the email integration
- The email integration sends an email to the DevOps team whenever the monitored site is down

Output integrations do not have any hard limits on responses. They can take as long as they need to process the data and send it to the external service. They do not have a `tick_url` endpoint, only a `target_url` endpoint. For good sanity, it is recommended that output integrations, like all other integrations, return a response within 1 second of receiving a message. They can do their processing in the background and send to whatever external service they need to send to. Telex does not expect a response body from them. Only a 202 status code is expected, which means the integration has accepted the data and will process it in the background.

## In Summary

- Modifier integrations modify new messages entering a channel and must return a response within 1 second of receiving a message. They do not have a `tick_url` endpoint, only a `target_url` endpoint.
- Interval integrations send messages to a channel at set intervals. They may or may not receive new messages entering the channel. They have a `target_url` and a `tick_url` endpoint. The `target_url` is where channel data can be sent to and the `tick_url` is where Telex can request data from the integration when its defined interval is reached. Most interval integrations will not need a `target_url` endpoint.
- Output integrations are like routers. They send data to external services whenever new data enters a Telex channel. They do not have a `tick_url` endpoint, only a `target_url` endpoint.
