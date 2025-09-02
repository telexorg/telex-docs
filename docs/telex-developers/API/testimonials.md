# Testimonials

## Overview
The **Telex Testimonials API** provides endpoints to manage testimonials within your application. This includes creating new testimonials and retrieving all testimonials. These endpoints ensure efficient handling of user feedback and experiences.

### Create a New Testimonial
- **Endpoint:** `POST /testimonials`
- **Summary:** Create a new testimonial
- **Description:** Creates a new testimonial.
- **Security:** 
  - `bearerAuth: []`

#### Request Body
- **Content-Type:** `application/json`
- **Required:** true
```json
{
  "company_name": "paystack",
  "content": "The service is fantastic, great experience."
}
```

#### Responses
- **201**: Testimonial created successfully.
```json
{
  "status": "success",
  "message": "Testimonial created successfully",
  "data": {
    "id": "f47ac10b-58cc-4372-a567-0e02b2c3d479",
    "user_id": "0b89cd08-57fc-40b0-aa17-ed6d95f43cfe",
    "company_name": "paystack",
    "image_url": "https://image.com",
    "name": "Charles Ugberaese",
    "content": "The service is fantastic, great experience.",
    "created_at": "2024-07-18T00:00:00Z"
  }
}
```
- **400**: Bad Request.
```json
{
  "status": "error",
  "message": "Bad request",
  "error": "Invalid input parameters"
}
```
- **401**: Authentication error.
```json
{
  "status": "error",
  "message": "Authentication error",
  "error": "Unauthorized access"
}
```
- **500**: Server error.
```json
{
  "status": "error",
  "message": "Internal server error",
  "error": "An unexpected error occurred"
}
```

---

### Get All Testimonials
- **Endpoint:** `GET /testimonials`
- **Summary:** Get all testimonials

#### Parameters
- Page Limit Parameter
- Limit Parameter

#### Responses
- **200**: A list of all testimonials.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Testimonials retrieved successfully.",
  "data": [
    {
      "id": "f47ac10b-58cc-4372-a567-0e02b2c3d479",
      "user_id": "0b89cd08-57fc-40b0-aa17-ed6d95f43cfe",
      "company_name": "paystack",
      "image_url": "https://image.com",
      "name": "Charles Ugberaese",
      "content": "The service is fantastic, great experience.",
      "created_at": "2024-07-18T00:00:00Z"
    }
    // Additional testimonials...
  ]
}
```
- **400**: Failed to fetch all testimonials.
```json
{
  "status": "error",
  "message": "Failed to fetch all testimonials",
  "error": "An error occurred while retrieving testimonials"
}
```
