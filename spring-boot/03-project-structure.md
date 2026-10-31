# Spring Boot Project Structure

A simple application to manage employees

```
employee-management/
├── pom.xml                  → maven configuration & dependencies
├── src/main/java/           → Java source code
│   ├── EmployeeApplication.java → Application entry point
│   ├── controller/          → Receives HTTP request
│   ├── service/             → Contains business logic
│   ├── repository/          → Access database
│   └── entity/              → Represent database entities
├── src/main/resources/      → configuration files & static resource
│   └── application.properties → DB URL, port & other settings
├── src/test/java/           → unit tests & integration tests
└── target/                  → Generated build output created by maven
```

## Layers

| Layer | Responsibility | Example |
|---|---|---|
| Controller | Accepts request | `GET /employees` |
| Service | Performs business logic | Calculate employee bonus |
| Repository | Communicates with DB | Find employee by ID |
| Entity | Represents stored data | Employee → ID, Name |

## Request Flow

```
              Client
        (Browser / Postman)
                 ↓
            Controller
     (Receives HTTP request)
                 ↓
              Service
     (Executes business logic)
                 ↓
            Repository
      (Reads or writes data)
                 ↓
             Database
     (Stores employee records)
```
