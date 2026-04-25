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

  
  

