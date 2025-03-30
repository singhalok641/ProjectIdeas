## Task Manager Pro

### **Overview**

You’re building a small-scale Task Management System for a hypothetical team or company. Each **Task** can be assigned to a **User** and associated with a **Project**. The system should allow for creating, reading, updating, and deleting (CRUD) records for all three entities: Tasks, Users, and Projects.

### **Key Entities**

1. **User**
    - **Fields:**
        - `id` (Long, primary key)
        - `username` (String, unique)
        - `fullName` (String)
        - `email` (String, unique)
        - `role` (String, e.g. “DEVELOPER”, “MANAGER”)
        - `createdAt` (LocalDateTime)
    - **Relationships:**
        - One-to-Many with `Task` (a user can have multiple tasks).

2. **Project**
    - **Fields:**
        - `id` (Long, primary key)
        - `name` (String, unique)
        - `description` (String, optional)
        - `startDate` (LocalDate)
        - `endDate` (LocalDate)
        - `status` (String, e.g. “ACTIVE”, “COMPLETED”, “ON_HOLD”)
    - **Relationships:**
        - One-to-Many with `Task` (a project can have multiple tasks).

3. **Task**
    - **Fields:**
        - `id` (Long, primary key)
        - `title` (String)
        - `description` (String, optional)
        - `priority` (String, e.g. “LOW”, “MEDIUM”, “HIGH”)
        - `status` (String, e.g. “OPEN”, “IN_PROGRESS”, “DONE”)
        - `createdAt` (LocalDateTime)
        - `updatedAt` (LocalDateTime)
    - **Relationships:**
        - Many-to-One with `User` (assignedTo).
        - Many-to-One with `Project`.

### **Core Requirements**

1. **Basic CRUD Endpoints**
    - **User Controller**
        - `POST /users` — Create a new user.
        - `GET /users` — Get all users (consider pagination if users can be large in number).
        - `GET /users/{id}` — Get a single user by ID.
        - `PUT /users/{id}` — Update user details.
        - `DELETE /users/{id}` — Delete a user.

    - **Project Controller**
        - `POST /projects` — Create a new project.
        - `GET /projects` — Get all projects.
        - `GET /projects/{id}` — Get project by ID.
        - `PUT /projects/{id}` — Update project info.
        - `DELETE /projects/{id}` — Delete a project.

    - **Task Controller**
        - `POST /tasks` — Create a new task (assign to a user and project).
        - `GET /tasks` — Get all tasks.
        - `GET /tasks/{id}` — Get task by ID.
        - `PUT /tasks/{id}` — Update task details (e.g., status, description, assigned user).
        - `DELETE /tasks/{id}` — Delete a task.

2. **Data Validation**
    - Validate important fields (e.g., `username` not empty, `email` properly formatted, `title` not empty).
    - Return meaningful error messages when validations fail.

3. **Error Handling**
    - Handle cases where entities don’t exist (throw `ResourceNotFoundException` or similar).
    - Return appropriate HTTP status codes (e.g., 404 for not found, 400 for bad request, 201 for created).

4. **Timestamps and Auditing**
    - Automatically set `createdAt` and `updatedAt` fields for `Task`.
    - Consider capturing `createdAt` for `User` or other relevant auditing fields.

5. **Database Integration**
    - Use Spring Data JPA with a relational database (e.g., MySQL or PostgreSQL).
    - Configure your `application.properties` or `application.yml` for DB connection.

6. **Project Structure**
    - Follow a standard Spring Boot structure:
        - `controllers` package for REST controllers.
        - `services` package for business logic.
        - `repositories` package for JPA repositories.
        - `models/entities` package for entity classes.
    - Keep your logic layered to separate concerns properly.

### **Bonus (Optional) Features**

- **Search & Filtering:**
    - Filter tasks by status, priority, or assigned user.
    - Filter projects by status (`ACTIVE`, `COMPLETED`, etc.).

- **Pagination & Sorting:**
    - Implement pagination (e.g., `GET /tasks?page=0&size=10`) to limit large results.
    - Sorting by creation date or priority.

- **Authentication & Role-Based Access:**
    - Implement Spring Security so that only certain roles (e.g., “MANAGER”) can create or delete projects.
    - Developers can read and update tasks but can’t necessarily delete projects.

- **Automated Tests:**
    - Use JUnit + Mockito (or another framework) to test repositories, services, and controllers.
    - Cover both happy paths and edge cases.

- **Continuous Integration Setup:**
    - Create a simple Jenkins pipeline or GitHub Actions workflow for building and testing your project.

---

## **What You’ll Learn**

1. **Spring Boot Fundamentals:** Project setup, application structure, and best practices.
2. **RESTful API Development:** Designing endpoints, handling requests and responses, and returning proper status codes.
3. **Data Persistence with JPA:** Mapping entities and relationships (One-to-Many, Many-to-One), using repositories, and writing queries.
4. **Validation & Error Handling:** Ensuring data integrity with annotations and custom exceptions.
5. **Business Logic & Service Layer:** Separating concerns between controllers, services, and repositories.
6. **Optional Advanced Concepts:** Spring Security for role-based authentication, integration testing, or CI/CD pipelines.

---

### **How to Approach This Project**

1. **Plan the Database Schema:** Sketch out your entities and relationships.
2. **Set Up Spring Boot:** Generate your project using [Spring Initializr](https://start.spring.io/) with dependencies like Spring Web, Spring Data JPA, and possibly Lombok or Spring Security if you plan to explore advanced features.
3. **Create Entities & Repositories:** Define your `User`, `Project`, and `Task` entities. Create corresponding JPA repositories for each.
4. **Implement Controllers & Services:** Build the business logic in your service layer, then expose it via REST controllers.
5. **Test & Validate:** Write unit tests, test API endpoints with Postman or cURL, and fix any issues.
6. **Enhance & Iterate:** Add optional features like search, filtering, or security as you gain confidence.

---

This project will give you hands-on experience with Spring Boot fundamentals in a realistic scenario. You’ll practice setting up a proper folder structure, learn how to handle entity relationships, and create a robust set of CRUD APIs—all skills you can showcase in real-world applications or interviews.