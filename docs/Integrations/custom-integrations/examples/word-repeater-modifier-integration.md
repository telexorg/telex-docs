# Modifier Custom Integration - Word Repeater

This page details the steps to build a simple modifier integration that repeats words. Given a string "Don! Well, look who we've got here.", and settings specifying the target words as "Don", and "Well", it should return "Don Don Don! Well Well Well, look who we've got here". It's silly but it showcases how a custom integration might be built.

---
## 1. Prerequisites

Before you begin, ensure you have the following:

- **Go**: Installed on your machine (version 1.16 or later). Follow the official [installation guide](https://golang.org/doc/install).
- **Telex Account**: Set up with access to webhook configuration for a channel.
- **Basic Knowledge**: Familiarity with HTTP servers, JSON handling, and Go programming.

---

## 2. API Reference

For modifier-type integrations,we need to define a **POST endpoint** that serves as the entry point for processing incoming messages. This endpoint should be specified as the `target_url` in the integration's JSON configuration. Every integration should expose an endpoint that accepts `channel_id` `settings`, and `message` in the request payload.

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

- `channel_id` _(string, required)_ – The unique identifier for the Telex channel.
- `settings` _(array, required)_ – List of formatting settings applied to the message:
  - `maxMessageLength` _(integer, required)_ – Maximum allowed length for the formatted message.
  - `repeatWords` _(string, required)_ – Comma-separated words that should be repeated in the message.
  - `noOfRepetitions` _(integer, required)_ – Number of times the words should be repeated.
- `message` _(string, required)_ – The original message to be formatted.

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

- `event_name` _(string)_ – Identifies the event type as `message_formatted`.
- `message` _(string)_ – The processed and formatted message.
- `status` _(string)_ – Processing status.
- `username` _(string)_ – Name of the service bot processing the message.

---

## 3. Integration Workflow

1. **Send a POST request** to `/format-message` with the message payload.
2. **Message formatting is applied** based on the defined settings.
3. **The formatted message is returned as response to Telex**. The response body contains the modifications made to the message.

---

## 4. Implementation Details

The service processes the message by:

1. Extracting settings from the request.
2. Applying transformations, including:
   - Truncating messages that exceed `maxMessageLength`.
   - Repeating words based on `repeatWords` and `noOfRepetitions`.
3. Retrieving the formatted message.

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

<!-- ## 8. External Communication

- The service sends formatted messages to Telex using the webhook endpoint: `https://ping.telex.im/v1/return/{channel_id}`.
- If the request fails, an error message is logged. -->

---

## 8. Example Usage

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

## 9. Setting Up Telex Integration

1. Go to the **Integrations** section in Telex.
2. Create a new **outbound** integration.
3. Provide a URL containing the JSON configuration for the integration.
4. Save the integration and retrieve the URL of the channel webhook.

### Sample Integration JSON Configuration

```json
{
	"data": {
		"author": "Micah Shallom",
		"date": {
			"created_at": "2025-02-13",
			"updated_at": "2025-02-13"
		},
		"descriptions": {
			"app_description": "A message formatter bot that processes incoming messages and sends back formatted responses.",
			"app_logo": "https://media.tifi.tv/telexbucket/public/logos/formatter.png",
			"app_name": "Message Formatter",
			"app_url": "https://txtformat.com/",
			"background_color": "#ffffff"
		},
		"integration_category": "Communication & Collaboration",
		"integration_type": "modifier",
		"is_active": true,
		"key_features": [
			"Receive messages from Telex channels.",
			"Format messages based on predefined templates or logic.",
			"Send formatted responses back to the channel.",
			"Log message formatting activity for auditing purposes."
		],
		"permissions": {
			"events": [
				"Receive messages from Telex channels.",
				"Format messages based on predefined templates or logic.",
				"Send formatted responses back to the channel.",
				"Log message formatting activity for auditing purposes."
			]
		},
		"settings": [
			{
				"default": 100,
				"label": "maxMessageLength",
				"required": true,
				"type": "number"
			},
			{
				"default": "world,happy",
				"label": "repeatWords",
				"required": true,
				"type": "multi-select"
			},
			{
				"default": 2,
				"label": "noOfRepetitions",
				"required": true,
				"type": "number"
			}
		],
		"target_url": "https://system-integration.telex.im/format-message",
		"tick_url": "https://system-integration.telex.im/format-message",
		"website": "https://telex.im"
	}
}
```

## Using the Integration

Add the integration json URL upon creation of the Integration






