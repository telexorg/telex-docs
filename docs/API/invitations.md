# Organisation Invitation

## Overview
The **Telex Organisation Invitation API** provides endpoints to manage invitations within your application. This includes creating and sending invitations, verifying invitation tokens, canceling invitations, and resending invitation emails. These endpoints ensure efficient handling of user invitations and onboarding processes.

### Create and Send Invitations
- **Endpoint:** `POST /invite`
- **Summary:** Create and send invitations
- **Description:** Sends invitations to the specified email addresses to join the organization.

#### Request Body
- **Content-Type:** `application/json`
- **Required:** true
```json
{
  "org_id": "01918214-55e5-791b-9ebd-fa8d92d69fa1",
  "emails": ["micahshallom@gmail.com", "micahshallom24@gmail.com"],
  "role": "member"
}
```

#### Responses
- **201**: Invitations created and sent successfully.
```json
{
  "status": "success",
  "status_code": 201,
  "message": "Invitations created successfully",
  "data": {
    "errors": [
      "An invitation has already been sent to micahshallom@gmail.com",
      "Error checking for telex presence: user with email micahshallom24@gmail.com does not exist"
    ],
    "invitations": [
      {
        "id": "019185b0-8fb4-767f-bd9d-d612e588f465",
        "email": "micahshallom24@gmail.com",
        "org_id": "01918214-55e5-791b-9ebd-fa8d92d69fa1",
        "status": "invited",
        "invite_token": "a351f47cea15f91ca16a3b18a04a8c22",
        "is_telex_user": false,
        "invitation_link": "http://127.0.0.1:8019/api/v1/invite/accept_org_invitation?org_id=01918214-55e5-791b-9ebd-fa8d92d69fa1&invitation_token=a351f47cea15f91ca16a3b18a04a8c22",
        "sent_at": "2024-08-24T19:40:26.548569958+01:00",
        "expires_at": "2024-08-26T19:40:26.548423408+01:00"
      }
    ]
  }
}
```

---

### Verify an Invitation Token
- **Endpoint:** `POST /invite/verify`
- **Summary:** Verify an invitation token
- **Security:** 
  - `bearerAuth: []`
- **Description:** This endpoint verifies an invitation token. Upon successful verification, it returns an access token and user details.

#### Request Body
- **Content-Type:** `application/json`
- **Required:** true
```json
{
  "token": "0d1f072a48c629397f20329d93ac8621"
}
```

#### Responses
- **200**: User invited successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "User invited successfully",
  "data": {
    "access_token": "eyJhbGciE....",
    "user": {
      "avatar_url": "",
      "created_at": "1723741256",
      "email": "blessing@gmail.com",
      "expires_in": "1723914056",
      "first_name": "Blessing@Gmail.Com",
      "fullname": "Blessing@Gmail.Com",
      "id": "019156fc-3a25-783c-bb6c-813aa4abd2e9",
      "is_onboarded": false,
      "is_verified": false,
      "last_name": "",
      "password": "telex-707385",
      "phone": "",
      "updated_at": "1723741256",
      "username": ""
    }
  }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.

---

### Cancel an Invitation
- **Endpoint:** `DELETE /invite/{invite_id}`
- **Summary:** Cancel an invitation
- **Description:** Deletes an invitation with the specified `invite_id`.

#### Parameters
- **invite_id**: The ID of the invitation to be canceled (path parameter).

#### Responses
- **200**: Invitation canceled successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Invitation cancelled successfully"
}
```

---

### Resend Invitation Emails
- **Endpoint:** `POST /invite/resend`
- **Summary:** Resend invitation emails
- **Description:** Resends invitations to the specified email addresses.

#### Request Body
- **Content-Type:** `application/json`
- **Required:** true
```json
{
  "emails": ["micahshallom1@gmail.com"],
  "org_id": "01918214-55e5-791b-9ebd-fa8d92d69fa1"
}
```

#### Responses
- **200**: Invitations sent successfully.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "success",
  "data": "Invitations sent successfully"
}
```