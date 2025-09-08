# Todo-Api


# RESTful API Plan: To-Do List Application


This document outlines the RESTful APIs for a To-Do List application built on the MERN stack.


## 1. Overview of CRUD Operations


The following APIs collectively provide the four basic functions of persistent storage: Create, Read, Update, and Delete (CRUD).


*   **Create:** New tasks are created via a `POST` request to `/api/tasks`. User accounts are created with a `POST` to `/api/users/register`.
*   **Read:** Tasks are read via `GET` requests, either to list all tasks (`/api/tasks`) or retrieve a specific one (`/api/tasks/:id`).
*   **Update:** Existing tasks are updated using a `PUT` request to `/api/tasks/:id`.
*   **Delete:** Tasks are removed with a `DELETE` request to `/api/tasks/:id`.


---


## 2. Authentication APIs


These APIs handle user registration and login. Authentication is crucial to ensure that users can only access and manage their own to-do lists.


### **2.1. Register User**
*   **Endpoint:** `POST /api/users/register`
*   **Method:** `POST`
*   **Purpose:** To create a new user account.
*   **Reasoning:** This is the entry point for a new user. It securely hashes the user's password before saving it to the database.
*   **Sample Request Body:**
    ```json
    {
      "username": "testuser",
      "email": "test@example.com",
      "password": "password123"
    }
    ```
*   **Sample Response (Success):**
    ```json
    {
      "message": "User registered successfully"
    }
    ```


### **2.2. Login User**
*   **Endpoint:** `POST /api/users/login`
*   **Method:** `POST`
*   **Purpose:** To authenticate a user and return a JSON Web Token (JWT).
*   **Reasoning:** This API validates user credentials. Upon success, it generates a JWT that must be included in the headers of subsequent requests to access protected routes (like managing tasks).
*   **Sample Request Body:**
    ```json
    {
      "email": "test@example.com",
      "password": "password123"
    }
    ```
*   **Sample Response (Success):**
    ```json
    {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
    }
    ```


---


## 3. Task Management APIs


These APIs are protected and can only be accessed by an authenticated user. They handle all operations related to managing tasks.


### **3.1. Create a Task**
*   **Endpoint:** `POST /api/tasks`
*   **Method:** `POST`
*   **Purpose:** To add a new task to the user's list.
*   **Reasoning:** This allows users to create new to-do items. The backend will associate the task with the logged-in user's ID.
*   **Sample Request Body:**
    ```json
    {
      "title": "Buy groceries",
      "description": "Milk, Bread, Cheese"
    }
    ```
*   **Sample Response (Success):**
    ```json
    {
      "_id": "60d5ecb3b3f0a36a4c8a8a8a",
      "user": "507f1f77bcf86cd799439011",
      "title": "Buy groceries",
      "description": "Milk, Bread, Cheese",
      "isCompleted": false,
      "createdAt": "2025-09-08T12:00:00.000Z"
    }
    ```


### **3.2. Get All Tasks**
*   **Endpoint:** `GET /api/tasks`
*   **Method:** `GET`
*   **Purpose:** To retrieve all tasks for the authenticated user.
*   **Reasoning:** This is the primary endpoint for the frontend to display the user's to-do list.
*   **Sample Request Body:** (None)
*   **Sample Response (Success):**
    ```json
    [
      {
        "_id": "60d5ecb3b3f0a36a4c8a8a8a",
        "user": "507f1f77bcf86cd799439011",
        "title": "Buy groceries",
        "isCompleted": false,
        "createdAt": "2025-09-08T12:00:00.000Z"
      },
      {
        "_id": "60d5eccfb3f0a36a4c8a8a8b",
        "user": "507f1f77bcf86cd799439011",
        "title": "Finish project report",
        "isCompleted": true,
        "createdAt": "2025-09-07T10:30:00.000Z"
      }
    ]
    ```


### **3.3. Update a Task**
*   **Endpoint:** `PUT /api/tasks/:id`
*   **Method:** `PUT`
*   **Purpose:** To modify an existing task (e.g., edit title, mark as complete).
*   **Reasoning:** This endpoint allows users to change the state or content of a task. The `:id` parameter identifies the specific task to update.
*   **Sample Request Body:**
    ```json
    {
      "title": "Buy organic groceries",
      "isCompleted": true
    }
    ```
*   **Sample Response (Success):**
    ```json
    {
      "_id": "60d5ecb3b3f0a36a4c8a8a8a",
      "user": "507f1f77bcf86cd799439011",
      "title": "Buy organic groceries",
      "description": "Milk, Bread, Cheese",
      "isCompleted": true,
      "createdAt": "2025-09-08T12:00:00.000Z"
    }
    ```


### **3.4. Delete a Task**
*   **Endpoint:** `DELETE /api/tasks/:id`
*   **Method:** `DELETE`
*   **Purpose:** To remove a task from the user's list.
*   **Reasoning:** Provides the ability to permanently delete a to-do item.
*   **Sample Request Body:** (None)
*   **Sample Response (Success):**
    ```json
    {
      "message": "Task removed successfully"
    }
    ```


---


## 4. Potential Implementation Challenges


1.  **Authentication & Authorization:** Implementing JWT-based security correctly is critical. Middleware must be written in Express.js to protect the task-related routes, ensuring that a user can only modify their own tasks and not those of another user. This involves validating the token and checking that the user ID in the token matches the owner of the requested task resource.


2.  **Error Handling:** A robust error handling strategy is needed. This includes sending appropriate HTTP status codes (e.g., 400 for bad requests, 401 for unauthorized, 404 for not found, 500 for server errors) and clear error messages to the client.


3.  **Input Validation:** All incoming data from the client must be validated to prevent malformed data from being saved to the database and to protect against potential security vulnerabilities like NoSQL injection. Libraries like `Joi` or `express-validator` are typically used for this purpose.



