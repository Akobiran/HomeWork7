## 📌 Campus Taskboard API
 - A Spring Boot REST API for managing tasks, built with Spring Boot, Spring Data JPA, and H2 Database.
 - This project demonstrates backend development concepts such as REST APIs, database persistence, validation, pagination, and layered architecture.

## 🚀 Features
- Create, Read, Update, Delete (CRUD) tasks
- Task status management (completed / incomplete)
- Priority system (LOW, MEDIUM, HIGH)
- Search tasks by keyword
- Pagination & sorting support
- Validation with meaningful error messages
- Auto timestamp tracking (createdAt, updatedAt)
- Preloaded sample data

## 🏗️ Architecture
- The project follows Layered Architecture (Separation of Concerns):
- Controller → Service → Repository → Database
 ## Components:
- Controller Layer
- Handles HTTP requests & responses
→
- Service Layer
- Contains business logic
→
- Repository Layer
- Handles database operations via JPA
→
- Entity (Model)
- Represents database table
→
- Application Entry Point
- Starts Spring Boot app
→
- Data Initializer
- Seeds database with sample data
## 🗄️ Database
- Uses H2 in-memory database
- Table: tasks
## Entity Fields:
| Field       | Type          | Description              |
| ----------- | ------------- | ------------------------ |
| id          | Integer       | Primary key              |
| title       | String        | Required (3–100 chars)   |
| description | String        | Optional (max 500 chars) |
| completed   | Boolean       | Default: false           |
| priority    | Enum          | LOW, MEDIUM, HIGH        |
| createdAt   | LocalDateTime | Auto-generated           |
| updatedAt   | LocalDateTime | Auto-updated             |

## ⚙️ How to Run
- Clone Repository
 ```bash
git clone <your-repo-url>
cd campus-taskboard
```
2. Run Application
```bash
./mvnw spring-boot:run
```
3. Server Starts At
- http://localhost:8080
  ## 📡 API Endpoints
 ## 🔹 Basic CRUD
 | Method | Endpoint          | Description     |
| ------ | ----------------- | --------------- |
| GET    | `/api/tasks`      | Get all tasks   |
| GET    | `/api/tasks/{id}` | Get task by ID  |
| POST   | `/api/tasks`      | Create new task |
| PUT    | `/api/tasks/{id}` | Update task     |
| DELETE | `/api/tasks/{id}` | Delete task     |

## 🔹 Filtering
| Endpoint                         | Description          |
| -------------------------------- | -------------------- |
| `/api/tasks/completed`           | Get completed tasks  |
| `/api/tasks/incomplete`          | Get incomplete tasks |
| `/api/tasks/priority/{priority}` | Filter by priority   |

# 🔹 Search
- GET /api/tasks/search?keyword=homework
 # 🔹 Pagination
 - GET /api/tasks/paginated?page=0&size=10&sortBy=id
  ##  📥 Example Request (POST)
  ```
  {
  "title": "Finish Assignment",
  "description": "Complete Spring Boot project",
  "completed": false,
  "priority": "HIGH"
}
```
## 🧠 Key Concepts Used
- Spring Boot – application framework
- REST API – HTTP communication
- Spring Data JPA – database abstraction
- Hibernate – ORM
- H2 Database – in-memory DB
- Validation API – input validation
- Pagination & Sorting – efficient data handling
- Lombok – reduces boilerplate code

  
  

