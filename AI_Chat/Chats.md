# AI Chat WebSocket API Documentation

This API allows clients to initiate AI-powered chat sessions via WebSocket and retrieve chat session reports via HTTP.

---

## Endpoint 1: Start AI Chat Session (With Session ID)

```websocket
[WS] {{wss_url}}ai_chat/?agent=1&session_id={{session_id}}&token={{token}}&chat_type=standard
```

### Authentication Required

- Yes (via `token` query parameter)

### Method

- `WebSocket`

### Query Parameters

| Name        | Type   | Required | Description                                         |
|-------------|--------|----------|-----------------------------------------------------|
| `agent`     | int    | Yes      | Agent ID (use `1` for default AI agent)            |
| `session_id`| string | Yes      | Unique session identifier for the chat             |
| `token`     | string | Yes      | Authentication token (Bearer)                      |
| `chat_type` | string | Yes      | Chat mode (`standard`, `report`, etc.)             |

---

### Success Message Format (WebSocket JSON Exchange)

**Send:**

```json
{
  "role": "user",
  "message": "Hello!"
}
```

**Receive:**

```json
{
  "role": "agent",
  "message": "Hi there! How can I assist you today?"
}
```

| Field   | Type   | Description        |
|---------|--------|--------------------|
| role    | string | "user" or "agent"  |
| message | string | Message content    |

---

### Common Error Responses

| Code | Meaning      | Description                          |
|------|--------------|--------------------------------------|
| 400  | Bad Request  | Missing or invalid query parameters  |
| 401  | Unauthorized | Token is missing or invalid          |
| 403  | Forbidden    | Token doesn’t grant access           |
| 404  | Not Found    | Session ID not found                 |

---

### Example (WebSocket)

```sh
wscat -c "{{wss_url}}ai_chat/?agent=1&session_id={{session_id}}&token={{token}}&chat_type=standard"
```

---

## Endpoint 2: Start AI Chat Session (Without Session ID)

```websocket
[WS] {{wss_url}}ai_chat/?agent=1&token={{token}}&chat_type=standard
```

### Use Case

Used when session_id is not yet generated; the backend may create and return a session automatically.

---

## Endpoint 3: Start AI Assistant (No Session, Direct Start)

```websocket
[WS] {{wss_url}}ai_assistant/?token={{token}}&agent=1
```

### Use Case

Initiate a direct chat session with the AI assistant using just the token and agent ID.

---

## Endpoint 4: Report Chat Session

```websocket
[WS] {{wss_url}}report_chat/?token={{token}}&project_id={{project_id}}&chat_type=report&report_type=systemreport&report_id=7
```

### Authentication Required
- Yes (via token query parameter)

### Query Parameters

| Name        | Type    | Required | Description                                      |
|-------------|---------|----------|--------------------------------------------------|
| token       | string  | Yes      | User authentication token (Bearer)              |
| project_id  | integer | Yes      | Project identifier                               |
| chat_type   | string  | Yes      | Must be `report`                                 |
| report_type | string  | Yes      | Type of report to retrieve (`systemreport`)      |
| report_id   | integer | Yes      | ID of the report to fetch                        |

---

### Success Response

```json
{
  "report_id": 7,
  "status": "completed",
  "summary": "The system has flagged unusual responses from the AI.",
  "generated_at": "2025-04-24T12:45:00Z"
}
```

| Field        | Type     | Description                                  |
|--------------|----------|----------------------------------------------|
| report_id    | integer  | Unique identifier for the report             |
| status       | string   | Status of the report (`completed`)           |
| summary      | string   | Textual summary of the report findings       |
| generated_at | datetime | Timestamp when the report was created        |

---

### Error Responses

| Code | Meaning      | Description                                 |
|------|--------------|---------------------------------------------|
| 400  | Bad Request  | Missing or invalid parameters               |
| 401  | Unauthorized | Invalid or missing token                    |
| 404  | Not Found    | Report or project ID doesn’t exist          |

---

### Example (cURL)

```sh
curl -X GET "{{wss_url}}report_chat/?token={{token}}&project_id=12&chat_type=report&report_type=systemreport&report_id=7"
```

---

### Notes
- WebSocket sessions require a valid token and agent ID.
- Session IDs are optional but recommended to track user-specific conversations.
- The reporting API is GET-only and retrieves existing reports.
- token must be valid and belong to an authenticated user.

---

### Related Endpoints
- POST /ai_chat/ – (future) create chat session via HTTP
- GET /report_chat/ – retrieve system-generated reports

---

### Rate Limiting
- Max: 100 requests per minute
- Status 429 Too Many Requests will be returned if exceeded.

