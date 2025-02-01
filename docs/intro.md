# Telex API Documentation

## Welcome to Telex

Welcome to the Telex API documentation! Telex is an all-in-one monitoring solution for DevOps and Software teams. You can utilize Telex to monitor databases, file systems, websites, and logs. Telex builds on a very simple idea — http webhooks that can receive data from anywhere. Telex is optimized for bulk data ingestion owing to its robust infastructure. This means Telex will not introduce any performance issue if it is integrated into your product.

### Getting Started

To get started with Telex, create an account at https://telex.im/auth/sign-up. You will be prompted to create an organization on signup. Once that is done, you should proceed to create your first channel and send your first webhook message. The gif below showcases the process of registering a new account on Telex and sending your first message.

![Signup and first message](/gif/signup-and-first-message.gif)

The gif below shows the process of creating your first channel and making your first request

![Create channel and send first webhooks](/gif/create-first-channel-and-send-first-webhook.gif)

Telex is designed for webhook events and retransmission. Imagine a user signup up on your system and you want that message appearing on Slack. Instead of writing a Slack integration yourself, you simply signup for Telex, create a channel, configure the default Slack integration, and voila, you get an easy to use system, exposed via API calls that can retrasmit events in your application.

There are tons of creative ways to use Telex and its beauty ultimately lies in the freedom of building custom integrations. You can build a simple app that connects to the Telex system to modify data get pinged at intervals. Get started by building your first integrations.
