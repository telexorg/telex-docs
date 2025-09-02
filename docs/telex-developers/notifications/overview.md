---
sidebar_position: 1
---

# Introduction

Telex Notifications enable real-time alerts for developers building **Telex clients** — applications that connect directly to Telex’s backend to receive updates, act or respond to system events. This guide explains how the notification system works, how to connect to it, and how to handle incoming events programmatically.

> ⚠️ This guide is intended for developers and technical integrators. It is **not** for general Telex users or agent creators using the UI.


## Conceptual Overview
The notification system is powered by **Centrifugo** and **WebSocket connections**. Once authenticated, the client subscribes to specific channels and receives push messages in real time.

### High-Level Flow

1. App starts and logs in using credentials from `env.json`
2. Retrieves organizations associated with the user
3. Requests subscription tokens for each channel
4. Establishes a WebSocket connection to Centrifugo
5. Receives and handles notifications in JSON format


### Purpose of Notifications

Notifications are designed to:

- Alert users to thread changes, new messages, or system events  
- Keep users informed even when the main Telex app is closed  
- Support native desktop integrations that surface alerts outside the browser or Flutter client  

This ensures users stay up to date without needing to keep the full Telex app open. Notifications are timely, relevant, and unobtrusive — enhancing productivity without interruption.

### How It Works for Users (Non-Technical Perspective)

From the user's point of view, the notiication app operates quietly in the background:

- They launch the Telex Start desktop app  
- The app connects to Telex and begins listening for backend updates  
- When something changes — like a new thread or reply — Telex Start displays a native desktop notification  