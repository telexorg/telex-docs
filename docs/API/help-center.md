# Help Center

## Overview
The **Help Center API** provides endpoints to manage help center categories and articles. This includes creating categories, retrieving categories, creating articles, and searching through articles.

### Create Help Center Category
- **Endpoint:** `/help-center/categories`
- **Method:** `POST`
- **Tags:** Help Center
- **Summary:** Create a help center category
- **Security:** 
    - `bearerAuth: []`

#### Request Body
```json
{
    "name": "Category Example",
    "description": "This is a test category"
}
```

#### Responses
- **201**: Help center category created successfully.
```json
{
    "status": "success",
    "statusCode": 201,
    "message": "Category added successfully",
    "data": {
        "category_id": "019150d5-1e95-707f-b73e-88e5c58db563",
        "name": "Category Example",
        "description": "This is a test category",
        "created_at": "2024-08-14T13:20:29.9743151+01:00",
        "updated_at": "2024-08-14T13:20:29.9743151+01:00"
    }
}
```
- **401**: Unauthenticated.
```json
{
    "status": "error",
    "message": "Unauthorized access"
}
```
- **500**: Server error.
```json
{
    "error": "Internal server error"
}
```

---

### Get All Help Center Categories
- **Endpoint:** `/help-center/categories`
- **Method:** `GET`
- **Tags:** Help Center
- **Summary:** Get all help center categories

#### Responses
- **200**: Help center categories retrieved successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Categories retrieved successfully.",
    "data": {
        "articles": [
            {
                "category_id": "019150d5-1e95-707f-b73e-88e5c58db563",
                "name": "Category Example",
                "description": "This is a test category",
                "created_at": "2024-08-14T13:20:29.9743151+01:00",
                "updated_at": "2024-08-14T13:20:29.9743151+01:00"
            }
            // More category objects...
        ],
        "pagination": {
            "current_page": 1,
            "page_size": 1,
            "total_items": 1,
            "total_pages": 1
        }
    }
}
```
- **404**: Category not found.
```json
{
    "error": "Category not found"
}
```
- **500**: Server error.
```json
{
    "error": "Internal server error"
}
```

---

### Create a New Help Center Article
- **Endpoint:** `/help-center/articles/{category_id}`
- **Method:** `POST`
- **Tags:** Help Center
- **Summary:** Create a new help center article
- **Security:** 
    - `bearerAuth: []`

#### Parameters
- **category_id** (path): The ID of the category to which the article belongs.

#### Request Body
```json
{
    "title": "Article Title",
    "content": "This is a test article"
}
```

#### Responses
- **201**: Help center article created successfully.
```json
{
    "status": "success",
    "statusCode": 201,
    "message": "Article added successfully",
    "data": {
        "article_id": "b8a1bcf2-d8f2-49d4-a42d-21ef9c03b305",
        "title": "Article Title",
        "content": "This is a test article",
        "category_id": "019150d5-1e95-707f-b73e-88e5c58db563",
        "created_at": "2024-08-14T13:20:29.9743151+01:00",
        "updated_at": "2024-08-14T13:20:29.9743151+01:00"
    }
}
```
- **400**: Bad request.
```json
{
    "error": "Bad request"
}
```
- **401**: Unauthenticated.
```json
{
    "status": "error",
    "message": "Unauthorized access"
}
```
- **404**: Category not found.
```json
{
    "error": "Category not found"
}
```
- **500**: Server error.
```json
{
    "error": "Internal server error"
}
```

---

### Get Paginated Articles by Category ID
- **Endpoint:** `/help-center/articles/categories/{category_id}`
- **Method:** `GET`
- **Tags:** Help Center
- **Summary:** Get paginated articles that belong to a particular category ID

#### Parameters
- **category_id** (path): The ID of the category.
- **page** (query): The page number to retrieve.
- **limit** (query): The number of articles per page.

#### Responses
- **200**: Paginated articles retrieved successfully.
```json
{
    "status": "success",
    "message": "Articles retrieved successfully",
    "statusCode": 200,
    "data": {
        "articles": [
            {
                "article_id": "b8a1bcf2-d8f2-49d4-a42d-21ef9c03b305",
                "title": "Article Title",
                "content": "This is a test article",
                "category_id": "019150d5-1e95-707f-b73e-88e5c58db563",
                "created_at": "2024-08-14T13:20:29.9743151+01:00",
                "updated_at": "2024-08-14T13:20:29.9743151+01:00"
            }
            // More article objects...
        ],
        "pagination": {
            "current_page": 1,
            "page_size": 1,
            "total_items": 1,
            "total_pages": 1
        }
    }
}
```
- **404**: Category not found.
```json
{
    "error": "Category not found"
}
```
- **500**: Server error.
```json
{
    "error": "Internal server error"
}
```

---

### Get Help Center Article by ID
- **Endpoint:** `/help-center/articles/{article_id}`
- **Method:** `GET`
- **Tags:** Help Center
- **Summary:** Get help center article by ID

#### Parameters
- **article_id** (path): The ID of the article.

#### Responses
- **200**: Article retrieved successfully.
```json
{
    "status": "success",
    "message": "Help center article retrieved successfully",
    "statusCode": 200,
    "data": {
        "article_id": "b8a1bcf2-d8f2-49d4-a42d-21ef9c03b305",
        "title": "Article Title",
        "content": "This is a test article",
        "category_id": "019150d5-1e95-707f-b73e-88e5c58db563",
        "created_at": "2024-08-14T13:20:29.9743151+01:00",
        "updated_at": "2024-08-14T13:20:29.9743151+01:00"
    }
}
```
- **404**: Article not found.
```json
{
    "error": "Article not found"
}
```
- **500**: Server error.
```json
{
    "error": "Internal server error"
}
```

---

### Search Help Center Articles
- **Endpoint:** `/help-center/articles/search`
- **Method:** `GET`
- **Tags:** Help Center
- **Summary:** Search Help Center articles
- **Description:** Searches Help Center topics by title.

#### Parameters
- **title** (query): The search topic title.

#### Responses
- **200**: Articles search results retrieved successfully.
```json
{
    "status": "success",
    "status_code": 200,
    "message": "Articles retrieved successfully.",
    "data": {
        "articles": [
            {
                "id": "0190fade-6a88-783f-97bc-870d0f5c187e",
                "title": "How to reset password",
                "content": "To reset your password, go to the settings page and click 'Reset Password'.",
                "author": "John Doe",
                "created_at": "2024-08-14T13:20:29.9743151+01:00",
                "updated_at": "2024-08-14T13:20:29.9743151+01:00"
            }
            // More article objects...
        ],
        "pagination": {
            "current_page": 1,
            "page_size": 1,
            "total_items": 1,
            "total_pages": 1
        }
    }
}
```