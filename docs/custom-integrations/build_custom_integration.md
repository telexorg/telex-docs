
# Setting up a custom integration

## Message Formatter Integration

This step-by-step guide will teach you how to build a simple **Message Formatter Integration** for Telex using the Go programming language. By the end of this tutorial, you'll have a server that receives messages from Telex, formats them, and sends a response back via a webhook.

---

## Prerequisites

Before you begin, make sure you have the following tools:

- **Go**: Installed on your machine (version 1.16 or later). Follow the official [installation guide](https://golang.org/doc/install).
- **Telex account**: Set up with access to webhook configuration for a channel.
- **Basic knowledge**: Familiarity with HTTP servers and JSON handling in Go.

---

## Setting Up Your Environment

First, let's set up the workspace and initialize a new Go module:

1. **Create a project folder**:

   ```bash
   mkdir message-formatter-bot
   cd message-formatter-bot
   ```

2. **Initialize the module**:

   ```bash
   go mod init message-formatter-bot
   ```

   This command creates a `go.mod` file that tracks your project dependencies.

3. **Create a main file**:

   We'll put our code in a file named `main.go`. Create it in the root of your project directory:

   ```bash
   touch main.go
   ```

   Now open `main.go` in your favorite code editor.

---

## Writing the HTTP Server

### Importing Packages

Start by defining the package and importing the necessary packages:

```go
package main

import (
	"bytes"
	"encoding/json"
	"fmt"
	"log"
	"net/http"
	"os"
)
```

These imports allow us to handle HTTP requests, encode and decode JSON, log messages, and access environment variables.

### Defining the Data Structure

We’ll define a struct to represent the incoming message payload from Telex:

```go
// Message represents the incoming payload structure
type Message struct {
	MessageContent struct {
		EventName string `json:"event_name"`
		Message   string `json:"message"`
		Status    string `json:"status"`
		Username  string `json:"username"`
	} `json:"message_content"`
	Settings struct {
		APIKey         string `json:"API_KEY"`
		OrganisationID string `json:"organisation_id"`
		ChannelID      string `json:"channel_id"`
	} `json:"settings"`
}
```

This struct matches the JSON payload that Telex sends. The nested structure keeps message content and settings logically separated.

---

### Implementing the HTTP Handler

The `handleIncomingMessage` function will process incoming POST requests:

```go
func handleIncomingMessage(w http.ResponseWriter, r *http.Request) {
	if r.Method != http.MethodPost {
		http.Error(w, "Invalid request method", http.StatusMethodNotAllowed)
		return
	}

	var msgReq Message
	if err := json.NewDecoder(r.Body).Decode(&msgReq); err != nil {
		http.Error(w, "Bad request", http.StatusBadRequest)
		return
	}

	formattedMessage := fmt.Sprintf("Hello, %s! You said: %s", msgReq.MessageContent.Username, msgReq.MessageContent.Message)
	log.Printf("Formatted message: %s", formattedMessage)

	response := map[string]string{
		"event_name": "message_formatted",
		"message":    formattedMessage,
		"status":     "success",
		"username":   "message-formatter-bot",
	}
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(response)

	sendMessageToTelex("https://ping.telex.im/v1/webhooks/your-webhook-id", msgReq.Settings.ChannelID, formattedMessage)
}
```

This function checks for a POST method, decodes the request body into a `Message` struct, and formats a greeting message using the provided username and message. It responds with a formatted message and sends the result back to Telex using a helper function.

---

### Sending Messages to Telex

The `sendMessageToTelex` function sends the formatted message to a webhook URL:

```go
func sendMessageToTelex(webhookURL, channelID, message string) {
	payload := map[string]string{"channel_id": channelID, "message": message}
	data, _ := json.Marshal(payload)

	resp, err := http.Post(webhookURL, "application/json", bytes.NewBuffer(data))
	if err != nil {
		log.Printf("Failed to send message: %v", err)
		return
	}
	defer resp.Body.Close()
	log.Printf("Message sent with status: %s", resp.Status)
}
```

This function creates a JSON payload with the channel ID and message, then sends a POST request to the webhook URL.

---

### Main Function

Finally, set up the main function to start the server:

```go
func main() {
	http.HandleFunc("/format-message", handleIncomingMessage)
	port := os.Getenv("PORT")
	if port == "" {
		port = "8080"
	}
	log.Printf("Server running on port %s", port)
	log.Fatal(http.ListenAndServe(":"+port, nil))
}
```

This function defines a route `/format-message` and starts the server on the specified port.

---

## Running the Server

To run your server:

1. **Set the `PORT` environment variable** (optional).
2. **Run the server**:

   ```bash
   go run main.go
   ```

You should see a message indicating the server is running.

---

## Setting Up Telex Integration

1. Go to the **Integrations** section in Telex.
2. Create a new **outbound** integration.
3. Set the **URL** to `http://your-server-url/format-message`.
4. Activate the integration.

---

## Testing the Integration

Send a message through Telex:

- **Input**: "Hello, Telex!"
- **Response**: "Hello, User! You said: Hello, Telex!"

---

## Conclusion
In this tutorial, you created a simple message formatter bot for Telex using Go. You learned how to create a custom integration, expose an endpoint that collects collects messages from a telex channel and also learnt how to send processed data back into a channel. Expand on this example by adding more logic or integrating additional features.

