---

## 2. Add New Email to Newsletter
- **Endpoint:** `/newsletter`
- **Method:** `POST`
- **Tags:** Newsletter
- **Summary:** Add a new email entry.

### Request Body
```json
{
  "email": "johndoe@example.com"
}
```

### Responses
- **201**: Email added successfully.
```json
{
  "status": "success",
  "status_code": 201,
  "message": "Email added successfully"
}
```
- **400**: Bad Request.
- **401**: Authentication error.
- **500**: Server error.
