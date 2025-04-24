# 🧾 Task App API Endpoint Documentation

This document provides detailed API endpoint documentation for the Task app, including User Tasks, Project Tasks, Company Tasks, Checklists, and Comments.

---

## Authentication

All endpoints require authentication. The user must be authenticated to access the APIs.

---

## 📝 User Tasks

### 1. Create User Task

**Method:** `POST`  
**URL:** `/create-usertask/`

#### Request Body
```json
{
  "title": "Task title",
  "description": "Task description",
  "status": "pending",
  "priority": "high",
  "duedate": "2024-12-31T23:59:59Z"
}
```

#### Response
```json
{
  "taskid": 123,
  "title": "Task title",
  "description": "Task description",
  "status": "pending",
  "priority": "high",
  "duedate": "2024-12-31T23:59:59Z",
  "createdAt": "2024-06-01T12:00:00Z",
  "updatedAt": "2024-06-01T12:00:00Z",
  "userid": {
    "id": 1,
    "email": "user@example.com"
  }
}
```

#### 🔁 Edge Cases

- If required fields are missing, the API returns a `400 Bad Request` with validation errors.
- If the user is not authenticated, the API returns a `401 Unauthorized` error.

---

### 2. Get User Tasks

**Method:** `GET`  
**URL:** `/user-tasks/`

#### Response
```json
[
  {
    "taskid": 123,
    "title": "Task title",
    "description": "Task description",
    "status": "pending",
    "priority": "high",
    "duedate": "2024-12-31T23:59:59Z",
    "createdAt": "2024-06-01T12:00:00Z",
    "updatedAt": "2024-06-01T12:00:00Z",
    "userid": {
      "id": 1,
      "email": "user@example.com"
    }
  }
]
```

---

### 3. Update User Task

**Method:** `PUT`  
**URL:** `/user-tasks/{task_id}/`

#### Request Body
Partial or full UserTask fields to update.

#### Response
Updated UserTask object.

---

## 📝 User Task Checklists

### 1. Create User Task Checklist

**Method:** `POST`  
**URL:** `/user-task-checklist/`

#### Request Body
```json
{
  "taskid": 123,
  "item": "Checklist item",
  "duedate": "2024-12-31T23:59:59Z"
}
```

#### Response
Created UserTaskCheckList object.

---

### 2. Get User Task Checklists

**Method:** `GET`  
**URL:** `/user-task-checklists/{task_id}/`

#### Response
List of UserTaskCheckList objects.

---

## 📝 User Task Comments

### 1. Create User Task Comment

**Method:** `POST`  
**URL:** `/user-task-comment/`

#### Request Body
```json
{
  "taskID": 123,
  "content": "This is a comment",
  "parentID": null
}
```

#### Response
Created UserTaskComments object.

---

## 📝 Project Tasks

### 1. Create Project Task

**Method:** `POST`  
**URL:** `/project-task/`

#### Request Body
```json
{
  "ProjectId": "uuid-string",
  "title": "Project task title",
  "description": "Description",
  "AssignedTo": [1, 2],
  "role": 1,
  "duedate": "2024-12-31T23:59:59Z"
}
```

#### Response
Created ProjectTask object.

---

### 2. Get Project Tasks

**Method:** `GET`  
**URL:** `/project-tasks/{project_id}/`

#### Response
List of ProjectTask objects.

---

### 3. Update Project Task

**Method:** `PUT`  
**URL:** `/project-tasks/{task_id}/`

#### Request Body
Partial or full ProjectTask fields.

#### Response
Updated ProjectTask object.

---

### 4. Delete Project Task

**Method:** `DELETE`  
**URL:** `/project-tasks/{task_id}/delete/`

#### Response
Success message.

---

## 📝 Project Task Checklists

### 1. Create Project Task Checklist

**Method:** `POST`  
**URL:** `/project-task-checklist/`

#### Request Body
```json
{
  "taskid": 123,
  "item": "Checklist item",
  "duedate": "2024-12-31T23:59:59Z"
}
```

#### Response
Created ProjectTaskCheckList object.

---

### 2. Get Project Task Checklists

**Method:** `GET`  
**URL:** `/project-task-checklists/{task_id}/`

#### Response
List of ProjectTaskCheckList objects.

---

### 3. Update Project Task Checklist

**Method:** `PUT`  
**URL:** `/checklist/{checklist_id}/`

#### Response
Updated ProjectTaskCheckList object.

---

### 4. Delete Project Task Checklist

**Method:** `DELETE`  
**URL:** `/project-tasks-checklist/{checklist_id}/delete/`

#### Response
Success message.

---

## 📝 Project Task Comments

### 1. Create Project Task Comment

**Method:** `POST`  
**URL:** `/project-task-comment/`

#### Request Body
```json
{
  "taskID": 123,
  "content": "Comment content",
  "parentID": null
}
```

#### Response
Created ProjectTaskComments object.

---

### 2. Get Project Task Comments

**Method:** `GET`  
**URL:** `/project-task-comments/{task_id}/`

#### Response
List of ProjectTaskComments objects.

---

### 3. Edit Project Task Comment

**Method:** `PUT`  
**URL:** `/project-task-edit-comments/{comment_id}/`

#### Response
Updated ProjectTaskComments object.

---

### 4. Delete Project Task Comment

**Method:** `DELETE`  
**URL:** `/project-tasks-comments/{comment_id}/delete/`

#### Response
Success message.

---

## 📝 Company Tasks

### 1. Create Company Task

**Method:** `POST`  
**URL:** `/company-tasks/`

#### Request Body
```json
{
  "CompanyId": 1,
  "title": "Company task title",
  "description": "Description",
  "AssignedTo": [1, 2],
  "role": 1,
  "duedate": "2024-12-31T23:59:59Z"
}
```

#### Response
Created CompanyTask object.

---

### 2. List Company Tasks

**Method:** `GET`  
**URL:** `/list-company-tasks/`

#### Query Parameters
- `CompanyId` (required)
- Optional filters: `due_date`, `priority`, `status`

#### Response
List of CompanyTask objects.

---

### 3. Update Company Task

**Method:** `PUT`  
**URL:** `/company-tasks/{task_id}/`

#### Response
Updated CompanyTask object.

---

### 4. Delete Company Task

**Method:** `DELETE`  
**URL:** `/company-tasks/delete/{taskid}/`

#### Response
Success message.

---

## 📝 Company Task Checklists and Comments

Similar endpoints exist for creating, listing, updating, and deleting checklists and comments for company tasks, following the same patterns as project tasks.

---

## 📝 Router Endpoints (v2)

The `ProjectTaskViewSet` is registered under `/v2/tasks/` with standard RESTful endpoints:

- GET `/v2/tasks/` - List tasks
- POST `/v2/tasks/` - Create task
- GET `/v2/tasks/{pk}/` - Retrieve task
- PUT `/v2/tasks/{pk}/` - Update task
- DELETE `/v2/tasks/{pk}/` - Delete task

---
