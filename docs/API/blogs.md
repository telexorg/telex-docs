# Blog 

## Overview
The **Telex Blog API** provides endpoints to manage blog posts and categories within your application. This includes creating, updating, retrieving, and deleting blog posts, as well as managing blog categories and submitting feedback.


### 1. Create a Blog Post
- **Endpoint:** `POST /blogs`
- **Summary:** Create a blog post by admin
- **Security:** bearerAuth
- **Request Body:**
  - **Content-Type:** `application/json`
  - **Required:** true
```json
{
  "content": "This is the content of the new blog post",
  "category_id": "01910544-d1e1-7ada-bdac-c761e527ec91"
}
```

#### Responses
- **201**: Blog created successfully.
```json
{
  "status": "success",
  "status_code": 201,
  "message": "Blog created successfully."
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **500**: Server error.

---

### 2. Get All Blogs
- **Endpoint:** `GET /blogs`
- **Summary:** Get all blogs from the latest
- **Parameters:**
  - **search** (query): The search query to match against blog titles and content.
  - **category** (query): The ID of the blog category to filter by.

#### Responses
- **200**: A list of all blogs.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Blogs retrieved successfully.",
  "data": [
    {
      "id": "019143a0-c5cc-7c8b-be12-7b8143c3c60a",
      "title": "Airplane crash due to stalling",
      "content": "Brazil is somewhere to pay something",
      "category_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
      "image_url": "https://brazil.com",
      "author_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
      "created_at": "2024-07-30T21:11:21.9538358+01:00",
      "updated_at": "2024-07-30T21:11:21.9538358+01:00"
    }
  ]
}
```
- **400**: Failed to fetch all blogs.

---

### 3. Retrieve a Single Blog Post
- **Endpoint:** `GET /blogs/{blogId}`
- **Summary:** Retrieve a single blog post
- **Parameters:**
  - **blogId** (path): ID of the blog to retrieve.

#### Responses
- **200**: Successful response.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Blog post retrieved successfully.",
  "data": {
    "id": "019143a0-c5cc-7c8b-be12-7b8143c3c60a",
    "title": "Airplane crash due to stalling",
    "content": "Brazil is somewhere to pay something",
    "category_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
    "image_url": "https://brazil.com",
    "author_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
    "created_at": "2024-07-30T21:11:21.9538358+01:00",
    "updated_at": "2024-07-30T21:11:21.9538358+01:00"
  }
}
```
- **400**: Bad request.
- **404**: Not found.
- **500**: Server error.

---

### 4. Delete a Blog Post
- **Endpoint:** `DELETE /blogs/{blogId}`
- **Summary:** Delete a blog by admin
- **Security:** bearerAuth
- **Parameters:**
  - **blogId** (path): ID of the blog to delete.

#### Responses
- **204**: Blog deleted successfully.
- **400**: Invalid blogId format.
- **401**: Unauthorized.
- **403**: Forbidden.
- **404**: Not found.
- **500**: Internal server error.

---

### 5. Create a Blog Category
- **Endpoint:** `POST /blog_categories`
- **Summary:** Create a blog category by admin
- **Security:** bearerAuth
- **Request Body:**
```json
{
  "name": "categoryName"
}
```

#### Responses
- **201**: Blog category created successfully.
```json
{
  "status": "success",
  "status_code": 201,
  "message": "Blog category created successfully.",
  "data": {
    "id": "01910544-d1e1-7ada-bdac-c761e527ec91",
    "name": "categoryName",
    "created_at": "2024-07-30T21:11:21.9538358+01:00",
    "updated_at": "2024-07-30T21:11:21.9538358+01:00"
  }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **500**: Server error.

---

### 6. Get All Blog Categories
- **Endpoint:** `GET /blog_categories`
- **Summary:** Get all blog categories.

#### Responses
- **200**: A list of all blog categories.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Blog categories retrieved successfully.",
  "data": [
    {
      "id": "01910544-d1e1-7ada-bdac-c761e527ec91",
      "name": "categoryName",
      "created_at": "2024-07-30T21:11:21.9538358+01:00",
      "updated_at": "2024-07-30T21:11:21.9538358+01:00"
    }
  ]
}
```
- **400**: Failed to fetch all blog categories.

---

### 7. Retrieve a Single Blog Category
- **Endpoint:** `GET /blog_categories/{categoryId}`
- **Summary:** Retrieve a single blog category
- **Parameters:**
  - **categoryId** (path): ID of the category to retrieve.

#### Responses
- **200**: Successful response.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Blog category retrieved successfully.",
  "data": {
    "id": "01910544-d1e1-7ada-bdac-c761e527ec91",
    "name": "News",
    "created_at": "2024-07-30T21:11:21.9538358+01:00",
    "updated_at": "2024-07-30T21:11:21.9538358+01:00"
  }
}
```
- **400**: Bad request.
- **404**: Not found.
- **500**: Server error.

---

### 8. Delete a Blog Category
- **Endpoint:** `DELETE /blog_categories/{categoryId}`
- **Summary:** Delete a blog category by admin
- **Security:** bearerAuth
- **Parameters:**
  - **categoryId** (path): ID of the category to delete.

#### Responses
- **204**: Blog category deleted successfully.
- **400**: Invalid categoryId format.
- **401**: Unauthorized.
- **403**: Forbidden.
- **404**: Not found.
- **500**: Internal server error.

---

### 9. Submit Blog Feedback
- **Endpoint:** `POST /blogs/feedback`
- **Summary:** Submit blog feedback
- **Request Body:**
```json
{
  "blog_id": "01910544-d1e1-7ada-bdac-c761e527ec91",
  "feedback": true
}
```

#### Responses
- **200**: Blog feedback submitted successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Blog feedback submitted successfully."
}
```
