# 📡 Connection API - Storyvord (Network URLs)

API collection for user connection and suggestion features using `/api/network/` base URL.

## 🔐 Authorization
All requests require a bearer token:
```http
Authorization: Bearer <access_token>
```

---

## 1. ✅ Check Email Exists

**Method:** `GET`  
**URL:**
```
{{api_base_url}}api/network/check-email?email=user@example.com
```

**Headers:**
```http
Authorization: Bearer <access_token>
```

**Response:**
```json
{
  "exists": true
}
```

---

## 2. ➕ Send Connection Request

**Method:** `POST`  
**URL:**
```
{{api_base_url}}api/network/connections/send
```

**Headers:**
```http
Content-Type: application/json  
Authorization: Bearer <access_token>
```

**Body (JSON):**
```json
{
  "receiver_email": "targetuser@example.com"
}
```

**Response:**
```json
{
  "id": 1,
  "requester": 5,
  "receiver": 8,
  "status": "pending",
  "created_at": "2024-01-01T12:00:00Z"
}
```

---

## 3. ⚙️ Manage Connection Request

**Method:** `POST`  
**URL:**
```
{{api_base_url}}api/network/connections/manage
```

**Headers:**
```http
Content-Type: application/json  
Authorization: Bearer <access_token>
```

**Body (JSON):**
```json
{
  "requester_id": 2,
  "status": "accepted"
}
```

**Response (on accept):**
```json
{
  "message": "Connection accepted and active connection created",
  "connection": {
    "id": 1,
    "requester": 2,
    "receiver": 5,
    "status": "accept",
    "created_at": "2024-01-01T12:00:00Z"
  },
  "active_connection": {
    "id": 10,
    "user_id_1": 2,
    "user_id_2": 5,
    "created_at": "2024-01-01T12:05:00Z"
  }
}
```

**Response (on decline):**
```json
{
  "id": 1,
  "requester": 2,
  "receiver": 5,
  "status": "declined",
  "created_at": "2024-01-01T12:00:00Z"
}
```

---

## 4. 👥 View My Connections

**Method:** `GET`  
**URL:**
```
{{api_base_url}}api/network/connections
```

**Headers:**
```http
Authorization: Bearer <access_token>
```

**Response:**
```json
[
  {
    "id": 5,
    "username": "johndoe",
    "email": "johndoe@example.com",
    "personalinfo": {
      "full_name": "John Doe",
      "bio": "Software developer",
      "location": "New York",
      "image": "https://example.com/media/profile.jpg"
    }
  }
]
```

---

## 5. 📩 View Connection Requests (Pending)

**Method:** `GET`  
**URL:**
```
{{api_base_url}}api/network/connections/requests?status=pending
```

**Headers:**
```http
Authorization: Bearer <access_token>
```

**Response:**
```json
[
  {
    "id": 1,
    "requester": 2,
    "receiver": 5,
    "status": "pending",
    "created_at": "2024-01-01T12:00:00Z"
  }
]
```

---

## 6. 🤝 Suggested Profiles

**Method:** `GET`  
**URL:**
```
{{api_base_url}}api/network/suggested-profiles
```

**Headers:**
```http
Authorization: Bearer <access_token>
```

**Response:**
```json
[
  {
    "user_id": 8,
    "full_name": "Jane Smith",
    "bio": "Data scientist",
    "location": "San Francisco",
    "image": "https://example.com/media/jane.jpg",
    "similarity": 0.85
  }
]
```
