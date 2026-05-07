## 📌 Campus Taskboard API

A Spring Boot REST API for managing tasks, built with Spring Boot, Spring Data JPA, and H2 Database.

This project demonstrates backend development concepts such as REST APIs, database persistence, validation, exception handling, logging, pagination, DTOs, and layered architecture.

---

##  Features

- Create, Read, Update, Delete (CRUD) tasks
- Task status management: completed / incomplete
- Priority system: LOW, MEDIUM, HIGH
- Search tasks by keyword
- Pagination & sorting support
- Validation with meaningful error messages
- Global exception handling
- Custom exceptions for missing or invalid task data
- DTO classes for request and response objects
- Request logging filter
- Auto timestamp tracking: createdAt and updatedAt
- H2 in-memory database support
- Preloaded sample data

---

##  Architecture

The project follows Layered Architecture, also known as Separation of Concerns.

```text
Controller → Service → Repository → Database
```

### Components

| Layer / Component | Responsibility |
| ----------------- | -------------- |
| Controller Layer | Handles HTTP requests and responses |
| Service Layer | Contains business logic |
| Repository Layer | Handles database operations using Spring Data JPA |
| Entity / Model | Represents the database table |
| DTO Classes | Control request and response data sent through the API |
| Exception Handler | Provides consistent error responses |
| Logging Filter | Logs incoming API requests, response status, and duration |
| Application Entry Point | Starts the Spring Boot application |
| Data Initializer | Seeds the database with sample data |

---

##  Database

The project uses an H2 in-memory database.

- Table name: `tasks`

### Entity Fields

| Field       | Type          | Description              |
| ----------- | ------------- | ------------------------ |
| id          | Integer       | Primary key              |
| title       | String        | Required, 3–100 chars    |
| description | String        | Optional, max 500 chars  |
| completed   | Boolean       | Default: false           |
| priority    | Enum          | LOW, MEDIUM, HIGH        |
| createdAt   | LocalDateTime | Auto-generated when task is created |
| updatedAt   | LocalDateTime | Auto-updated when task is modified |
| deleted     | Boolean       | Used to mark whether a task is deleted |

### JPA Notes

- `@Entity` tells Spring/JPA that the `Task` class represents a database table.
- `@Id` marks the primary key.
- `@GeneratedValue(strategy = GenerationType.IDENTITY)` lets the database generate the task ID.
- `@Column` is used to control database column rules such as length and nullable values.
- `@Enumerated(EnumType.STRING)` stores the priority enum as text instead of a number.
- `@PrePersist` sets `createdAt` and `updatedAt` before the task is first saved.
- `@PreUpdate` updates `updatedAt` whenever the task is modified.

---

##  DTO Classes

The project uses DTOs to separate API input/output from the database entity.

### `TaskRequest`

Used when creating a task. It includes:

- `title`
- `description`
- `completed`
- `priority`

Validation rules are applied here, such as requiring a title and limiting the title size.

### `TaskResponse`

Used when returning a task after creation. It includes clean response data such as:

- `id`
- `title`
- `description`
- `completed`
- `priority`
- `createdAt`
- `updatedAt`

This prevents the API from exposing unnecessary internal details.

### `ErrorResponse`

Used for structured error messages. It returns:

- `timestamp`
- `status`
- `error`
- `message`
- `path`

---

##  How to Run

### 1. Clone Repository

```bash
git clone <your-repo-url>
cd campus-taskboard
```

### 2. Run Application

```bash
./mvnw spring-boot:run
```

### 3. Server Starts At

```text
http://localhost:8080
```

---

##  API Endpoints

###  Basic CRUD

| Method | Endpoint          | Description     |
| ------ | ----------------- | --------------- |
| GET    | `/api/tasks`      | Get all active tasks |
| GET    | `/api/tasks/{id}` | Get task by ID  |
| POST   | `/api/tasks`      | Create new task |
| PUT    | `/api/tasks/{id}` | Update task     |
| DELETE | `/api/tasks/{id}` | Delete task     |

###  Filtering

| Method | Endpoint                         | Description          |
| ------ | -------------------------------- | -------------------- |
| GET    | `/api/tasks/completed`           | Get completed tasks  |
| GET    | `/api/tasks/incomplete`          | Get incomplete tasks |
| GET    | `/api/tasks/priority/{priority}` | Filter by priority   |

###  Search

```http
GET /api/tasks/search?keyword=homework
```

This endpoint searches tasks by matching the keyword against the task title or description.

###  Pagination

```http
GET /api/tasks/paginated?page=0&size=10&sortBy=id
```

Query parameters:

| Parameter | Meaning |
| --------- | ------- |
| page | Page number, starting at 0 |
| size | Number of tasks per page |
| sortBy | Field used for sorting, such as `id`, `title`, or `priority` |

---

##  Example Request: Create Task

```http
POST /api/tasks
Content-Type: application/json
```

```json
{
  "title": "Finish Assignment",
  "description": "Complete Spring Boot project",
  "completed": false,
  "priority": "HIGH"
}
```

### Example Successful Response

```json
{
  "id": 1,
  "title": "Finish Assignment",
  "description": "Complete Spring Boot project",
  "completed": false,
  "priority": "HIGH",
  "createdAt": "2026-05-01T12:30:00",
  "updatedAt": "2026-05-01T12:30:00"
}
```

---

##  Validation

The API validates incoming task data before saving it.

Validation rules include:

| Field | Rule |
| ----- | ---- |
| title | Required |
| title | Must be between 3 and 100 characters |
| description | Cannot exceed 500 characters |

If the request body is invalid, the API returns a `400 Bad Request` response with validation details.

### Example Invalid Request

```json
{
  "title": "Hi",
  "description": "Invalid because the title is too short",
  "priority": "HIGH"
}
```

### Example Validation Error Response

```json
{
  "timestamp": "2026-05-01T12:30:00",
  "status": 400,
  "error": "Validation Failed",
  "message": "Input validation failed",
  "errors": {
    "title": "Title must be between 3 and 100 characters"
  },
  "path": "/api/tasks"
}
```

---

## Exception Handling

The project includes a global exception handler using `@RestControllerAdvice`.

This keeps error handling separate from the controller and makes error responses more consistent.

### Custom Exceptions

| Exception | When It Happens | HTTP Status |
| --------- | --------------- | ----------- |
| `TaskNotFoundException` | A task ID does not exist | `404 Not Found` |
| `InvalidTaskDataException` | Task data is invalid | `400 Bad Request` |
| `MethodArgumentNotValidException` | Validation annotations fail | `400 Bad Request` |
| `ConstraintViolationException` | A constraint is violated | `400 Bad Request` |
| Generic `Exception` | Unexpected server error | `500 Internal Server Error` |

### Example Not Found Response

```json
{
  "timestamp": "2026-05-01T12:30:00",
  "status": 404,
  "error": "Not Found",
  "message": "Task with ID 99 not found",
  "path": "/api/tasks/99"
}
```

---

##  Logging

The project includes a custom `LoggingFilter`.

The logging filter records:

- HTTP method
- Request URI
- Response status code
- Request duration in milliseconds

Example log output:

```text
GET /api/tasks - Status: 200 - Duration: 15ms
```

This helps with debugging and monitoring API requests.

---

##  Repository Methods

The repository extends `JpaRepository<Task, Integer>`, so Spring Data JPA automatically provides CRUD methods such as:

- `save()`
- `findById()`
- `findAll()`
- `deleteById()`
- `existsById()`

The project also uses custom repository methods based on Spring Data JPA naming conventions:

| Method | Purpose |
| ------ | ------- |
| `findByCompletedTrue()` | Finds completed tasks |
| `findByCompletedFalse()` | Finds incomplete tasks |
| `findByPriority()` | Finds tasks by priority |
| `findByTitleContainingIgnoreCase()` | Searches by title without case sensitivity |
| `findByCompletedAndPriority()` | Finds tasks by status and priority |
| `findByDeletedFalse()` | Finds tasks that are not marked deleted |
| `findByDeletedTrue()` | Finds tasks marked deleted |
| `countByPriority()` | Counts tasks by priority |

The project also includes a custom JPQL query for keyword search:

```java
@Query("SELECT t FROM Task t WHERE t.title LIKE %:keyword% OR t.description LIKE %:keyword%")
List<Task> searchTasks(@Param("keyword") String keyword);
```

---

##  Key Concepts Used

- Spring Boot – application framework
- REST API – HTTP communication
- Spring Data JPA – database abstraction
- Hibernate – ORM
- H2 Database – in-memory database
- Validation API – input validation
- DTO pattern – separates API data from entity data
- Global exception handling – centralized API error responses
- Custom exceptions – clearer error logic
- Logging filter – request/response logging
- Pagination & Sorting – efficient data handling
- Lombok – reduces boilerplate code
- Layered Architecture – separates controller, service, repository, and model responsibilities

---

##  Video Link

- https://youtu.be/Ask8xs9Ca_U?si=NFg7mfsbkyrB6HU1

