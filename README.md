# Campus Taskboard API

A simple Spring Boot REST API for managing tasks. This project allows users to create, view, update, and delete tasks through HTTP requests. It is designed as a beginner-friendly backend project to demonstrate REST APIs, controller-service architecture, validation, and JSON request/response handling.

## Features

- Create a new task
- View all tasks
- View a task by ID
- Update an existing task
- Delete a task
- Validate user input
- Return proper HTTP status codes
- Store tasks in memory during runtime

## Tech Stack

- Java
- Spring Boot
- Maven
- Lombok
- Jakarta Validation
- Postman for API testing
## How the Project Works
- This project follows a simple layered structure:
- Task: the model class that represents task data
- TaskService: contains the business logic for storing and managing tasks
- TaskController: handles HTTP requests and sends HTTP responses
- When a client sends a request, the controller receives it, calls the service, and returns the result as JSON.
## Task Fields
- Each task contains:
- id – unique task ID
- title – required, 3 to 100 characters
- description – optional, maximum 500 characters
- completed – task status (true or false)
- priority – task priority (LOW, MEDIUM, or HIGH)
## Validation Rules
- The API validates incoming task data:
- title cannot be blank
- title must be between 3 and 100 characters
- description cannot exceed 500 characters
- If validation fails, the API returns:
- 400 Bad Request
- with a JSON response describing the error.
## Server URL
By default, the application runs at: http ://localhost:8080
## API ENDPOINTS 
## Get all tasks 
- Request: GET api/tasks
- Response: 200 OK
  Example :
  ```
  {
    "id": 1,
    "title": "Complete Homework 5",
    "description": "Finish Spring Boot API assignment",
    "completed": false,
    "priority": "HIGH"
  }
  ```
  ## GET TASK BY ID :
  - Request : GET api/task/{id}
   Example: GET api/task/1
    Responses
  - 200 OK if task exists
  - 404 Not Found if task does not exist
 Responses
- 200 OK if task exists
- 404 Not Found if task does not exist
  ## Create a task
  Request
  - POST: /api/tasks
  - Content-Type: application/json
  - Example body:
```{
  "title": "Complete Homework 5",
  "description": "Finish Spring Boot API assignment",
  "completed": false,
  "priority": "HIGH"
}
```
 ## Response
- 201 Created
- Example response:
```{
  "id": 1,
  "title": "Complete Homework 5",
  "description": "Finish Spring Boot API assignment",
  "completed": false,
  "priority": "HIGH"
}
```
## Update a task
Request
PUT /api/tasks/{id}
Content-Type: application/json
Example:
```{
  "title": "Complete Homework 5",
  "description": "Updated task description",
  "completed": true,
  "priority": "MEDIUM"
}
```
- Responses:
200 OK if updated successfully
404 Not Found if task does not exist
## Delete a task
 Request: DELETE /api/tasks/{id}
- Responses: 
- 204 No Content if the task was deleted
- 404 Not Found if the task does not exist
## Testing with Postman
- You can test the API using Postman.
- Example POST Request
- Method: POST
- URL: http://localhost:8080/api/tasks
- Header: Content-Type: application/json
- Body:
```{
  "title": "Study for exam",
  "description": "Review calculus notes",
  "completed": false,
  "priority": "HIGH"
}
```
## Learning Goals
- This project demonstrates:
- building a REST API with Spring Boot
- using HTTP methods such as GET, POST, PUT, and DELETE
- converting JSON into Java objects
- organizing code into model, service, and controller layers
- validating user input
- testing endpoints with Postman


  

  
  

