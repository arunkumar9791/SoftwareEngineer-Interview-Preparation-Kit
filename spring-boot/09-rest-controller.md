# Creating REST APIs using `@RestController` in Spring Boot

Let's understand this step by step with a simple example.

## 1. What is a REST API?

A REST API allows two applications to communicate with each other over HTTP.

For example, imagine you are building a student management application.

- A frontend application requests student details.
- The Spring Boot backend receives the request.
- The backend processes it and returns the student details as JSON.

```
Frontend Application
(Requests student details)
        ↓
Spring Boot REST API
(Receives request and processes it)
        ↓
Database (optional)
(Stores student information)
```

The REST API sends the response back to the frontend, usually in JSON format.

Example response:

```json
{
  "id": 101,
  "name": "Arun",
  "course": "Java"
}
```

## 2. What is `@RestController`?

`@RestController` is a Spring annotation used to create a controller that handles HTTP requests and returns data directly in the response body, usually as JSON.

In simple terms:

`@RestController` receives requests from clients and sends responses back to them.

Example:

```java
@RestController
public class StudentController {

    @GetMapping("/student")
    public String getStudent() {
        return "Welcome to Spring Boot";
    }
}
```

When a client sends a GET request to `/student`, the response is:

```
Welcome to Spring Boot
```

### What happens behind the scenes?

- `@RestController` tells Spring that this class handles web requests and returns response data.
- `@GetMapping("/student")` maps GET requests for `/student` to the method.
- `getStudent()` executes when that request is received.
- The returned string becomes the HTTP response body.

## 3. Important annotations used to create REST APIs

| Annotation | Simple meaning |
|---|---|
| `@RestController` | Defines a REST API controller. |
| `@RequestMapping` | Defines a common or specific URL mapping. |
| `@GetMapping` | Handles GET requests, typically to retrieve data. |
| `@PostMapping` | Handles POST requests, typically to create data. |
| `@PutMapping` | Handles PUT requests, typically to update data. |
| `@PatchMapping` | Handles PATCH requests, typically to update part of a resource. |
| `@DeleteMapping` | Handles DELETE requests, typically to delete data. |
| `@PathVariable` | Reads a value from the URL path. |
| `@RequestParam` | Reads a query parameter from the URL. |
| `@RequestBody` | Reads data sent in the request body. |

## 4. Practical example — Student REST API

Let's create a simple API that retrieves student information and adds a student.

### Step 1: Create the controller

```java
import org.springframework.web.bind.annotation.*;
import java.util.Map;

@RestController
@RequestMapping("/students")
public class StudentController {

    @GetMapping
    public String getAllStudents() {
        return "List of all students";
    }

    @GetMapping("/{id}")
    public String getStudentById(
            @PathVariable int id) {
        return "Student ID: " + id;
    }

    @PostMapping
    public String addStudent(
            @RequestBody Map<String, Object> student) {
        return "Student added: " + student.get("name");
    }
}
```

This is a simplified learning example. It does not save students in a database.

### Step 2: Understand each API

| Method | Purpose | URL | Returns |
|---|---|---|---|
| GET | Get all students | `/students` | `List of all students` |
| GET | Get a student by ID | `/students/101` | `Student ID: 101` |
| POST | Add a student | `/students` | Sends student data in the request body. |

### Step 3: Understand `@PathVariable`

Consider this URL:

```
GET /students/101
```

The method is:

```java
@GetMapping("/{id}")
public String getStudentById(@PathVariable int id) {
    return "Student ID: " + id;
}
```

Here:

- `/{id}` defines a variable portion of the URL.
- `@PathVariable` extracts the value from that portion.
- The value `101` is assigned to the Java variable `id`.

Therefore, the response is:

```
Student ID: 101
```

### Step 4: Understand `@RequestBody`

Suppose the client sends a POST request to `/students` with this JSON:

```json
{
  "id": 101,
  "name": "Arun"
}
```

The controller receives the request:

```java
@PostMapping
public String addStudent(
        @RequestBody Map<String, Object> student) {
    return "Student added: " + student.get("name");
}
```

`@RequestBody` tells Spring to read the incoming JSON and convert it into a Java object — here, a `Map`.

The response becomes:

```
Student added: Arun
```

## 5. How is `@RestController` different from `@Controller`?

| `@Controller` | `@RestController` |
|---|---|
| Commonly used for MVC web pages. | Commonly used for REST APIs. |
| A returned string is generally treated as a view name. | A returned string is written to the response body. |
| Use `@ResponseBody` when you want a method to return response data directly. | Response-body behavior is included by default. |

Remember: `@RestController` is effectively a combination of `@Controller` and `@ResponseBody`.

## 6. How do you test a REST API?

You can test APIs using Postman or a similar HTTP client. You can also open a GET endpoint in your browser.

For the example above, start the Spring Boot application and try:

| HTTP method | URL |
|---|---|
| GET | `http://localhost:8080/students` |
| GET | `http://localhost:8080/students/101` |
| POST | `http://localhost:8080/students` |

For the POST request in Postman, choose Body → raw → JSON and send:

```json
{
  "id": 101,
  "name": "Arun"
}
```

Make sure your Spring Boot application is running. These URLs assume the default port `8080`.

## Quick revision notes

- REST API: Allows applications to communicate through HTTP.
- `@RestController`: Handles HTTP requests and returns response data.
- `@GetMapping`: Retrieves data.
- `@PostMapping`: Creates or submits data.
- `@PutMapping`: Replaces or updates a resource.
- `@DeleteMapping`: Deletes a resource.
- `@PathVariable`: Gets values from the URL path.
- `@RequestParam`: Gets values from URL query parameters.
- `@RequestBody`: Reads data from the request body.

The most important concept to understand is that the client sends an HTTP request, Spring maps it to a controller method, and the method's return value becomes the HTTP response.
