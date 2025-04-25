# Notification WebSocket API Documentation

This API allows clients to receive real-time notifications via WebSocket and retrieve notification history and unread counts via HTTP.

---

## WebSocket Endpoint: Notification Channel

```websocket
[WS] {{wss_url}}notifications/?token={{token}}
```

### Authentication Required
- Yes (via `token` query parameter)

### Description
Establishes a WebSocket connection for receiving real-time user-specific notifications. Each user joins a personal group based on their ID after token authentication.

### Connection Parameters

| Name   | Type   | Required | Description                        |
|--------|--------|----------|------------------------------------|
| token  | string | Yes      | JWT token for authenticating user  |

### Message Format (Incoming from Server)
```json
{
  "message": "You have a new notification."
}
```

| Field   | Type   | Description                     |
|---------|--------|---------------------------------|
| message | string | The content of the notification |

### Error Responses

| Code | Meaning      | Description                       |
|------|--------------|-----------------------------------|
| 401  | Unauthorized | Missing or invalid token         |
| 403  | Forbidden    | User not authorized for this action |

---

## HTTP Endpoint: List Notifications

```http
GET /api/notification/notifications/
```

### Description
Retrieves a list of all notifications for the authenticated user.

### Authentication Required
- Yes (via Authorization header: `Bearer <token>`)

### Response Example
```json
[
  {
    "id": 1,
    "message": "Task deadline approaching!",
    "read": false,
    "timestamp": "2025-04-24T12:00:00Z"
  }
]
```

---

## HTTP Endpoint: Unread Notification Count

```http
GET api/notification/notifications/unread-count/
```

### Description
Retrieves the number of unread notifications for the authenticated user.

### Authentication Required
- Yes (via Authorization header: `Bearer <token>`)

### Response Example
```json
{
  "count": 5
}
```

---

## Notes
- Only authenticated users can access notification data.
- WebSocket connections must include a valid token in the query string.
- Notifications are sent in real-time using WebSocket protocol when triggered by backend events.

---

## Related WebSocket URL
- `{{wss_url}}notifications/?token={{token}}`

