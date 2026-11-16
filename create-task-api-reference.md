# Create Task API Reference

A REST API reference for creating a task in a project management
application.

## Endpoint

  Property             Value
  -------------------- --------------------------------------
  **Method**           `POST`
  **Path**             `/api/v1/projects/{projectId}/tasks`
  **Authentication**   Bearer access token
  **Content type**     `application/json`
  **Success status**   `201 Created`

### Description

Creates a new task in the specified project for an authenticated user
who has permission to create tasks in that project. The caller supplies
the task title, assignee, due date, and priority. A description may also
be supplied.

When creation succeeds, the API returns the new task, including its
generated ID, initial status, and timestamps.

## Authentication and request headers

Send the following headers with every request:

  ------------------------------------------------------------------------
  Header                                    Required Description
  --------------------- ---------------------------- ---------------------
  `Authorization`                                Yes Bearer access token
                                                     used to authenticate
                                                     the caller.

  `Content-Type`                                 Yes Must be
                                                     `application/json`.

  `Accept`                                       Yes Indicates that the
                                                     client expects a JSON
                                                     response.
  ------------------------------------------------------------------------

Example:

``` http
Authorization: Bearer <access_token>
Content-Type: application/json
Accept: application/json
```

The authenticated user must have permission to create tasks in the
target project.

## Request parameters

### Path parameters

  ------------------------------------------------------------------------
  Name             Type                          Required Description
  ---------------- ---------------- --------------------- ----------------
  `projectId`      string (UUID)                      Yes Unique
                                                          identifier of
                                                          the project in
                                                          which the task
                                                          will be created.

  ------------------------------------------------------------------------

### Query parameters

This endpoint does not define any query parameters.

### Request body

The request body must be a JSON object.

  ------------------------------------------------------------------------
  Name             Type                          Required Description
  ---------------- ---------------- --------------------- ----------------
  `title`          string                             Yes Task title. Must
                                                          contain 1--200
                                                          characters and
                                                          must not be
                                                          blank.

  `description`    string                              No Additional task
                                                          details. Maximum
                                                          length: 2,000
                                                          characters. If
                                                          omitted, the
                                                          description is
                                                          stored as
                                                          `null`.

  `assigneeId`     string (UUID)                      Yes ID of an
                                                          existing user
                                                          who is a member
                                                          of the specified
                                                          project.

  `dueDate`        string (date)                      Yes Task deadline in
                                                          `YYYY-MM-DD`
                                                          format (ISO 8601
                                                          full-date).

  `priority`       string (enum)                      Yes Task priority.
                                                          Allowed values:
                                                          `low`, `medium`,
                                                          `high`.
  ------------------------------------------------------------------------

### Validation and behaviour

-   `projectId` must identify an existing project.
-   `title` must be a non-empty string of no more than 200 characters.
-   `description`, when supplied, must not exceed 2,000 characters.
-   `assigneeId` must identify an existing user who belongs to the
    project.
-   `dueDate` must be a valid calendar date in `YYYY-MM-DD` format.
-   `priority` must be exactly `low`, `medium`, or `high`.
-   Newly created tasks receive the initial status `pending`.
-   The server generates `taskId`, `createdAt`, and `updatedAt`; clients
    must not supply these fields.

## Example request

``` http
POST /api/v1/projects/550e8400-e29b-41d4-a716-446655440000/tasks HTTP/1.1
Host: api.projectmanager.com
Authorization: Bearer <access_token>
Content-Type: application/json
Accept: application/json
```

``` json
{
  "title": "Prepare project progress report",
  "description": "Compile project activities, milestones, and achievements for the monthly review.",
  "assigneeId": "123e4567-e89b-12d3-a456-426614174000",
  "dueDate": "2026-11-30",
  "priority": "high"
}
```

## Responses

### `201 Created` --- Task created

The task was successfully created. The response body contains the newly
created task.

``` json
{
  "success": true,
  "message": "Task created successfully.",
  "data": {
    "taskId": "987e6543-e21b-45d3-a456-426614174999",
    "projectId": "550e8400-e29b-41d4-a716-446655440000",
    "title": "Prepare project progress report",
    "description": "Compile project activities, milestones, and achievements for the monthly review.",
    "assigneeId": "123e4567-e89b-12d3-a456-426614174000",
    "dueDate": "2026-11-30",
    "priority": "high",
    "status": "pending",
    "createdAt": "2026-11-16T09:30:00Z",
    "updatedAt": "2026-11-16T09:30:00Z"
  }
}
```

### Error responses

  ------------------------------------------------------------------------------
  Status code                    Meaning                 When it occurs
  ------------------------------ ----------------------- -----------------------
  `400 Bad Request`              Invalid request         The request is
                                                         malformed or a required
                                                         field is missing, has
                                                         the wrong type, or
                                                         violates basic input
                                                         formatting rules.

  `401 Unauthorized`             Authentication required The access token is
                                 or failed               missing, invalid, or
                                                         expired.

  `403 Forbidden`                Access denied           The authenticated user
                                                         does not have
                                                         permission to create
                                                         tasks in the project.

  `404 Not Found`                Resource not found      The project or
                                                         referenced assignee
                                                         cannot be found.

  `409 Conflict`                 Resource state conflict The project exists but
                                                         cannot accept new
                                                         tasks, for example
                                                         because it is archived.

  `415 Unsupported Media Type`   Unsupported content     The request body is not
                                 type                    sent as
                                                         `application/json`.

  `422 Unprocessable Content`    Semantic validation     The JSON is
                                 failed                  syntactically valid,
                                                         but a business rule is
                                                         not satisfied, such as
                                                         the assignee not being
                                                         a member of the
                                                         project.

  `429 Too Many Requests`        Rate limit exceeded     The caller has sent
                                                         more requests than the
                                                         API allows within the
                                                         configured rate-limit
                                                         window.

  `500 Internal Server Error`    Server error            An unexpected
                                                         server-side error
                                                         prevented the request
                                                         from completing.

  `503 Service Unavailable`      Service unavailable     The API is temporarily
                                                         unable to process
                                                         requests, for example
                                                         during maintenance or
                                                         overload.
  ------------------------------------------------------------------------------

### Error response format

The following is an illustrative error body. The exact error message may
vary by status code.

``` json
{
  "success": false,
  "message": "Request validation failed.",
  "error": {
    "code": "VALIDATION_ERROR",
    "details": [
      {
        "field": "priority",
        "message": "Priority must be one of: low, medium, high."
      }
    ]
  }
}
```

## Successful response fields

  -----------------------------------------------------------------------
  Field                   Type                    Description
  ----------------------- ----------------------- -----------------------
  `success`               boolean                 Indicates whether the
                                                  operation succeeded.

  `message`               string                  Human-readable result
                                                  message.

  `data.taskId`           string (UUID)           Server-generated unique
                                                  task identifier.

  `data.projectId`        string (UUID)           Project containing the
                                                  task.

  `data.title`            string                  Task title.

  `data.description`      string or `null`        Additional task
                                                  details; `null` when no
                                                  description was
                                                  supplied.

  `data.assigneeId`       string (UUID)           ID of the assigned
                                                  project member.

  `data.dueDate`          string (date)           Task deadline in
                                                  `YYYY-MM-DD` format.

  `data.priority`         string (enum)           Task priority: `low`,
                                                  `medium`, or `high`.

  `data.status`           string                  Initial task status,
                                                  set to `pending`.

  `data.createdAt`        string (date-time)      Creation timestamp in
                                                  ISO 8601 format,
                                                  expressed in UTC.

  `data.updatedAt`        string (date-time)      Last-update timestamp
                                                  in ISO 8601 format,
                                                  expressed in UTC.
  -----------------------------------------------------------------------

## Implementation notes

-   This is a proposed API contract for a hypothetical service; the host
    name, UUIDs, and sample values are illustrative.
-   The API should return `201 Created` only after the task has been
    persisted successfully.
-   The API should validate that the assignee belongs to the project
    before creating the task.
-   Clients should treat the server-generated fields as read-only.
-   For production documentation, align the error schema and exact
    status-code behaviour with the implemented service.

## Quick reference

``` text
POST /api/v1/projects/{projectId}/tasks
Authorization: Bearer <access_token>
Content-Type: application/json
Accept: application/json
```

Required body fields: `title`, `assigneeId`, `dueDate`, `priority`.

Optional body field: `description`.

Allowed priorities: `low`, `medium`, `high`.
