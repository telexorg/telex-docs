---
sidebar_position: 1
---

# Telex Integrations Overview

Telex is an integrations marketplace. The platform is built to allow developers to write scripts or build simple executables that collect data from a source and post it to a [Telex channel](/docs/Channels/intro). An example integration is a Summarizer that collects data from a Telex channel over a set period of time and posts a summary of all messages in that period back to the channel. Another example is a Bitcoin price tracker that uses the coingecko API to get the current price of Bitcoin and posts it every 30 minutes. A third example is a profanity filter that masks profane words in every new message entering a channel. On analyzing the examples above, it is clear that there are different categories of integrations. Broadly speaking, there are two categories:

1. Modifier integrations: These modify new messages entering a channel and are not time bound
2. Interval integrations: These send messages to a channel at their set intervals. They may or may not receive new messages entering the channel.

Below is a better explanation of the two categories.

## Modifier Integrations

Modifier integrations are integrations that modify new messages entering a channel. They are not time bound and do not send messages to the channel at set intervals. Examples of modifier integrations include: Profanity filter, capitalizer, text translator, etc. A hard limit of 1 second is set for modifier integrations, so this means they must return a response within 1 second of receiving a message, else their contribution is skipped.

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

## Technical Specs of Integration

All integrations in Telex follow a standardized data format for receiving information. The data is passed in a generic format that can be interpreted by any programming language:

```json
{
  "message": string,
  "channel_meta": {
    "name": string,
    "webhook_url": URL
  }
  "settings": Record<string, any>[]
}
```

The `Record<string, any>[]` is a list of key-value pairs that can be used to pass additional information to the integration. The message field is the message that the integration is expected to process. The settings field is a list of key-value pairs that can be used to pass additional information to the integration. The settings field is optional and can be omitted if not needed, althought this is rare.
