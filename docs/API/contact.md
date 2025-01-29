# Contact

## Overview
The **Telex Contact API** provides endpoints to manage contact messages within your application. This includes adding new messages to the contact us list. These endpoints ensure efficient handling of user inquiries and feedback.

### Add a New Message
- **Endpoint:** `POST /contact`
- **Summary:** Add a new message
- **Description:** Adds a new message to the contact us list.

#### Request Body
- **Content-Type:** `application/json`
- **Required:** true
```json
{
  "name": "John Doe",
  "email": "johndoe@example.com",
  "phone_number": "090567892",
  "message": "I would like to know more about your services1 with html content here</br><script>?</script>.3"
}
```

#### Responses
- **201**: Contact added successfully.
```json
{
  "status": "success",
  "status_code": 201,
  "message": "Contact added successfully."
}
```
- **400**: Bad Request.
```json
{
  "status": "error",
  "status_code": 400,
  "message": "Bad request",
  "error": "Invalid input parameters"
}
```
- **422**: Unprocessable Entity.
```json
{
  "status": "error",
  "status_code": 422,
  "message": "Unprocessable Entity",
  "error": "Validation failed for input data"
}
```
- **500**: Server error.
```json
{
  "status": "error",
  "status_code": 500,
  "message": "Internal server error",
  "error": "An unexpected error occurred"
}
```
