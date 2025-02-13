# Setting Up a Custom Integration

## Message Formatting Integration Documentation

## Overview

The **Message Formatting Integration** is a [**modifier-type custom integration**](../custom-integrations/creating_integration.md#modifier-integration-type) that enables developers to process and modify incoming messages before sending them to a designated Telex channel. This service applies custom formatting rules defined in the request payload, such as limiting message length, repeating specified words, and dynamically modifying message content.

This documentation is designed for **developers** and provides a comprehensive guide to setting up, implementing, and using the integration. It includes API reference, implementation details, deployment instructions, and examples.

---

## Table of Contents

1. **Prerequisites**
2. **API Reference**
3. **Integration Workflow**
4. **Implementation Details**
5. **Error Handling**
6. **Deployment & Environment Configuration**
7. **Logging**
8. **External Communication**
9. **Example Usage**
10. **Setting Up Telex Integration**
11. **Conclusion**

---

## 1. Prerequisites

Before you begin, ensure you have the following:

- **Go**: Installed on your machine (version 1.16 or later). Follow the official [installation guide](https://golang.org/doc/install).
- **Telex Account**: Set up with access to webhook configuration for a channel.
- **Basic Knowledge**: Familiarity with HTTP servers, JSON handling, and Go programming.

---

## 2. API Reference

For modifier-type integrations, you need to define a **POST endpoint** that serves as the entry point for processing incoming messages. This endpoint should be specified as the `target_url` in the integration's JSON configuration.

### Endpoint: Format Message

- **URL:** `/format-message`
- **Method:** `POST`
- **Content-Type:** `application/json`

#### Request Payload

```json
{
  "channel_id": "0192dd70-cdf1-7e15-8776-4fee4a78405e",
  "settings": [
    {
      "label": "maxMessageLength",
      "type": "number",
      "description": "Set the maximum length for incoming messages to format.",
      "default": 30,
      "required": true
    },
    {
      "label": "repeatWords",
      "type": "multi-select",
      "description": "Set the words that need to be repeated.",
      "default": "world, happy",
      "required": true
    },
    {
      "label": "noOfRepetitions",
      "type": "number",
      "description": "Set the number of repetitions for words that need to be repeated.",
      "default": 2,
      "required": true
    }
  ],
  "message": "Hello, world. I hope you are happy today"
}
```

#### Request Parameters

- `channel_id` *(string, required)* – The unique identifier for the Telex channel.
- `settings` *(array, required)* – List of formatting settings applied to the message:
  - `maxMessageLength` *(integer, required)* – Maximum allowed length for the formatted message.
  - `repeatWords` *(string, required)* – Comma-separated words that should be repeated in the message.
  - `noOfRepetitions` *(integer, required)* – Number of times the words should be repeated.
- `message` *(string, required)* – The original message to be formatted.

#### Response Payload

```json
{
  "event_name": "message_formatted",
  "message": "Hello, world world. I hope you are happy happy today",
  "status": "success",
  "username": "message-formatter-bot"
}
```

#### Response Parameters

- `event_name` *(string)* – Identifies the event type as `message_formatted`.
- `message` *(string)* – The processed and formatted message.
- `status` *(string)* – Processing status.
- `username` *(string)* – Name of the service bot processing the message.

---

## 3. Integration Workflow

1. **Send a POST request** to `/format-message` with the message payload.
2. **Message formatting is applied** based on the defined settings.
3. **A response is returned** containing the formatted message.
4. **The formatted message is sent to Telex** via `https://ping.telex.im/v1/return/{channel_id}`.

---

## 4. Implementation Details

The service processes the message by:

1. Extracting settings from the request.
2. Applying transformations, including:
   - Truncating messages that exceed `maxMessageLength`.
   - Repeating words based on `repeatWords` and `noOfRepetitions`.
3. Sending the formatted message back to the client and forwarding it to Telex.

### Implementing the HTTP Handler

The `handleIncomingMessage` function processes incoming POST requests:

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

	formattedMessage := settingsProcessing(msgReq)
	log.Printf("Formatted message: %s", formattedMessage)

	response := map[string]string{
		"event_name": "message_formatted",
		"message":    formattedMessage,
		"status":     "success",
		"username":   "message-formatter-bot",
	}
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(response)

	sendMessageToTelex("https://ping.telex.im/v1/return/", msgReq.ChannelID, response)
}
```

### Sending Messages to Telex

The `sendMessageToTelex` function sends the formatted message to a webhook URL:

```go
func sendMessageToTelex(webhookURL, channelID string, message map[string]string) {
	payload := map[string]any{"channel_id": channelID, "message": message}
	data, _ := json.Marshal(payload)

	resp, err := http.Post(fmt.Sprintf("%s%s", webhookURL, channelID), "application/json", bytes.NewBuffer(data))
	if err != nil {
		log.Printf("Failed to send message: %v", err)
		return
	}
	defer resp.Body.Close()
	log.Printf("Message sent with status: %s", resp.Status)
}
```

### Settings Processing

The `settingsProcessing` function applies the formatting settings to the message:

```go
func settingsProcessing(msgReq Message) string {
	maxMessageLength := 500
	var repeatWords []string
	noOfRepetitions := 1

	// Extract settings
	for _, setting := range msgReq.Settings {
		switch setting.Label {
		case "maxMessageLength":
			if val, ok := setting.Default.(float64); ok {
				maxMessageLength = int(val)
			}
		case "repeatWords":
			if val, ok := setting.Default.(string); ok {
				repeatWords = strings.Split(val, ", ")
			}
		case "noOfRepetitions":
			if val, ok := setting.Default.(float64); ok {
				noOfRepetitions = int(val)
			}
		}
	}

	formattedMessage := msgReq.Message

	// Repeat specified words
	for _, word := range repeatWords {
		formattedMessage = strings.ReplaceAll(formattedMessage, word, strings.Repeat(word+" ", noOfRepetitions))
	}
	// Apply maxMessageLength constraint
	if len(formattedMessage) > maxMessageLength {
		formattedMessage = formattedMessage[:maxMessageLength]
	}

	return formattedMessage
}
```

### Main Function

Set up the main function to start the server:

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

---

## 5. Error Handling

- **400 Bad Request:** Invalid JSON payload or missing required fields.
- **405 Method Not Allowed:** Only `POST` requests are accepted.
- **500 Internal Server Error:** Unexpected server error during processing.

---

## 6. Deployment & Environment Configuration

- The server runs on port **8080** by default but can be configured using the `PORT` environment variable.
- Run the service using:
  ```sh
  PORT=5000 go run main.go
  ```

---

## 7. Logging

Logs formatted messages and errors using `log.Printf` for debugging and monitoring purposes.

---

## 8. External Communication

- The service sends formatted messages to Telex using the webhook endpoint: `https://ping.telex.im/v1/return/{channel_id}`.
- If the request fails, an error message is logged.

---

## 9. Example Usage

### Sample cURL Request

```sh
curl -X POST "http://localhost:8080/format-message" \
     -H "Content-Type: application/json" \
     -d '{
       "channel_id": "0192dd70-cdf1-7e15-8776-4fee4a78405e",
       "settings": [
         {"label": "maxMessageLength", "type": "number", "default": 30, "required": true},
         {"label": "repeatWords", "type": "multi-select", "default": "world, happy", "required": true},
         {"label": "noOfRepetitions", "type": "number", "default": 2, "required": true}
       ],
       "message": "Hello, world. I hope you are happy today"
     }'
```

This request will return:

```json
{
  "event_name": "message_formatted",
  "message": "Hello, world world. I hope you are happy happy today",
  "status": "success",
  "username": "message-formatter-bot"
}
```

---

## 10. Setting Up Telex Integration

1. Go to the **Integrations** section in Telex.
2. Create a new **outbound** integration.
3. Provide a URL containing the JSON configuration for the integration.
4. Save the integration and retrieve the URL of the channel webhook.

### Sample Integration JSON Configuration

```json
{
  "date": {
    "created_at": "2025-01-27",
    "updated_at": "2025-01-27"
  },
  "descriptions": {
    "app_description": "A message formatter bot that processes incoming messages and sends back formatted responses.",
    "app_logo": "https://example.com/message-formatter-logo.png",
    "app_name": "Message Formatter",
    "app_url": "https://example.com/message-formatter",
    "background_color": "#0000FF"
  },
  "target_url": "https://<server-url>/format-message",
  "key_features": [
    "Receive messages from Telex channels.",
    "Format messages based on predefined templates or logic.",
    "Send formatted responses back to the channel.",
    "Log message formatting activity for auditing purposes."
  ],
  "settings": [
    {
      "label": "maxMessageLength",
      "type": "number",
      "description": "Set the maximum length for incoming messages to format.",
      "default": 30,
      "required": true
    },
    {
      "label": "repeatWords",
      "type": "multi-select",
      "description": "Set the words that need to be repeated.",
      "default": "world, happy",
      "required": true
    },
    {
      "label": "noOfRepetitions",
      "type": "number",
      "description": "Set the number of repetitions for words that need to be repeated.",
      "default": 2,
      "required": true
    }
  ],
  "is_active": true
}
```

---

## 11. Conclusion

This documentation provides all necessary details for developers to integrate and utilize the **Telex Message Formatting Integration** efficiently. Ensure that your request payloads are structured correctly to avoid errors. Happy coding! Love from Telex ❤️.

--- 

