---
sidebar_position: 0
---

# Welcome to Telex

Welcome to the Telex API documentation! Telex is an all-in-one monitoring solution for DevOps and Software teams. You can utilize Telex to monitor databases, file systems, websites, and logs. Telex builds on a very simple idea — HTTP webhooks that can receive data from anywhere. Telex is optimized for bulk data ingestion owing to its robust infrastructure. This means Telex will not introduce any performance issues if it is integrated into your product.

## The Core of Telex

The fundamental unit of the Telex platform is a channel. Everything message that goes in or out of Telex must pass through a channel. Users in a Telex organisation can be added or removed from channels, making them useful for separating concerns. A channel can serve as a routing mechanism to direct messages like application errors to your team's communication platform like Slack or Microsoft teams. You could also just use a channel directly for regular team chat and not have to pay for other tools. There are several other details about channels that if discussed would fill out this introduction, so it's best to continue reading about it on the [dedicated channel docs page](/docs/Channels/intro.md).

## Getting Started

To get started with Telex, create an account at https://telex.im/auth/sign-up. You will be prompted to create an organization upon signup. Once that is done, you should proceed to create your first channel and send your first webhook message.

### Messages on Telex channels

Telex by default can serve as a chat application for teams. Once a message enters a channel, all team members present in that channel receive the message in real time. This also means that when a third-party like an external system calling the channel's webhook or a custom integration sends a message to the channel, all subscribed members and tools receive the message in real time.

The gif below showcases the process of registering a new account on Telex and sending your first message.

![Signup and first message](/gif/signup-and-first-message.gif)

###

The gif below shows the process of creating your first channel and making your first request.

![Create channel and send first webhooks](/gif/create-first-channel-and-send-first-webhook.gif)

Telex is designed for webhook events and retransmission. Imagine a user signing up on your system and you want that event to appear on Slack. Instead of writing a creating a Slack app, and integrating Slack into your application, which is labourious, you simply sign up for Telex, create a channel, configure the default Slack integration, and call the webhook! Telex's design allows you to collect any type of event from your application and send them to downstream systems like Slack, Discord, Teams, etc.

There are tons of creative ways to use Telex, and its beauty ultimately lies in the freedom of building custom integrations. You can build a simple app that connects to the Telex system and modify data coming into channels. You can also build an app that can transmit data to Telex at set intervals. Telex maintains the timing and your app simply responds when Telex calls it. Get started by building your first integration.
