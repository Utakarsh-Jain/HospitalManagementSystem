# Hospital Management System – Spring Boot & JWT

## Project Overview

Hospital Management System is a backend REST API application developed using Java and Spring Boot. The application provides secure user authentication and patient record management through RESTful APIs.

The project uses Spring Security and JSON Web Tokens (JWT) to authenticate users and protect patient management endpoints. Spring Data JPA is used for database operations, while MySQL stores user credentials and patient information.

The application follows a layered architecture that separates controllers, services, repositories, entities, DTOs, and security configuration to maintain clean, modular, and maintainable code.

The system provides two primary modules:

1. **Authentication Module:** User registration, login, password encryption, JWT generation, and token-based authentication.
2. **Patient Management Module:** Adding, retrieving, updating, and deleting patient records through secured REST APIs.

## Project Objectives

- Develop a RESTful backend application using Spring Boot.
- Implement user registration and login functionality.
- Securely store user passwords using BCrypt password hashing.
- Generate and validate JWT tokens for authentication.
- Implement role-based authentication information using Spring Security.
- Perform patient management operations using CRUD APIs.
- Integrate MySQL using Spring Data JPA and Hibernate.
- Implement layered architecture for better code organization.
- Test business logic using JUnit 5 and Mockito.
- Verify REST APIs using Postman.

## Technologies Used

| Technology | Purpose |
|---|---|
| Java | Core programming language |
| Spring Boot | Backend application framework |
| Spring Web | REST API development |
| Spring Security | Authentication and API security |
| JWT | Token-based authentication |
| Spring Data JPA | Database interaction |
| Hibernate | ORM implementation |
| MySQL | Relational database |
| Maven | Dependency and build management |
| Lombok | Boilerplate code reduction |
| Jakarta Validation | Request data validation |
| JUnit 5 | Unit testing |
| Mockito | Mocking dependencies during testing |
| Postman | REST API testing |
| IntelliJ IDEA | Development environment |

## System Architecture

The application follows a layered architecture where each layer has a specific responsibility.

### 1. Controller Layer

The controller layer handles incoming HTTP requests and returns appropriate HTTP responses.

It connects the client with the service layer.

Controllers:

- `AuthController`
- `PatientController`

Responsibilities:

- Accept HTTP requests.
- Read request bodies and path variables.
- Perform request validation using Jakarta Validation.
- Call the appropriate service methods.
- Return HTTP responses with appropriate status codes.

### 2. Service Layer

The service layer contains the application's business logic.

Services:

- `AuthService`
- `PatientService`

Responsibilities:

- Register new users.
- Check for duplicate email addresses.
- Encrypt passwords before saving them.
- Authenticate users during login.
- Generate JWT tokens after successful authentication.
- Perform patient CRUD operations.
- Convert entities into DTOs and DTOs into entities.
- Handle records that do not exist.

### 3. Repository Layer

The repository layer communicates with the MySQL database using Spring Data JPA.

Repositories:

- `UserRepository`
- `PatientRepository`

Responsibilities:

- Save records.
- Retrieve records.
- Update records.
- Delete records.
- Find users using their email addresses.
- Check whether an email address is already registered.

Both repositories extend `JpaRepository`, which provides commonly used database operations without requiring manual SQL queries for basic CRUD functionality.

### 4. Entity Layer

The entity layer represents database tables.

Entities:

- `User`
- `Patient`
- `Role` (Enum)

The entities are mapped to database tables using Jakarta Persistence annotations.

### 5. DTO Layer

DTO stands for Data Transfer Object.

DTOs are used to transfer data between the client and the application without directly exposing database entities.

DTO classes:

- `AuthRequest`
- `AuthResponse`
- `PatientDto`

### 6. Security and Configuration Layer

This layer manages authentication, authorization, JWT operations, and application security.

Classes:

- `SecurityConfig`
- `JwtAuthenticationFilter`
- `JwtService`

Responsibilities:

- Configure Spring Security.
- Allow public access to authentication endpoints.
- Protect patient management endpoints.
- Generate JWT tokens.
- Extract user information from tokens.
- Validate token signatures and expiration.
- Authenticate requests using the JWT authentication filter.
- Maintain stateless session management.

## Project Structure

```text
hospital-management-system/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── hms/
│   │   │               │
│   │   │               ├── config/
│   │   │               │   ├── SecurityConfig.java
│   │   │               │   ├── JwtService.java
│   │   │               │   └── JwtAuthenticationFilter.java
│   │   │               │
│   │   │               ├── controller/
│   │   │               │   ├── AuthController.java
│   │   │               │   └── PatientController.java
│   │   │               │
│   │   │               ├── dto/
│   │   │               │   ├── AuthRequest.java
│   │   │               │   ├── AuthResponse.java
│   │   │               │   └── PatientDto.java
│   │   │               │
│   │   │               ├── entity/
│   │   │               │   ├── User.java
│   │   │               │   ├── Patient.java
│   │   │               │   └── Role.java
│   │   │               │
│   │   │               ├── repository/
│   │   │               │   ├── UserRepository.java
│   │   │               │   └── PatientRepository.java
│   │   │               │
│   │   │               ├── service/
│   │   │               │   ├── AuthService.java
│   │   │               │   └── PatientService.java
│   │   │               │
│   │   │               └── HospitalManagementSystemApplication.java
│   │   │
│   │   └── resources/
│   │       └── application.properties
│   │
│   └── test/
│       └── java/
│           └── com/
│               └── example/
│                   └── hms/
│                       ├── config/
│                       │   ├── JwtServiceTest.java
│                       │   └── JwtAuthenticationFilterTest.java
│                       │
│                       ├── controller/
│                       │   ├── AuthControllerTest.java
│                       │   └── PatientControllerTest.java
│                       │
│                       └── service/
│                           ├── AuthServiceTest.java
│                           └── PatientServiceTest.java
│
├── pom.xml
└── README.md
```

## Application Modules

### Module 1: Authentication

The authentication module provides user registration and login functionality.

#### User Registration

Endpoint:

`POST /api/auth/register`

The user provides the following details:

- Name
- Email
- Password

Registration workflow:

1. The client sends registration details through a POST request.
2. The controller receives and validates the request.
3. The service checks whether the email already exists.
4. The password is encrypted using BCrypt.
5. A new user entity is created.
6. The default `USER` role is assigned.
7. The user details are saved in MySQL.
8. A success response is returned.

Example request:

```json
{
    "name": "Admin User",
    "email": "admin@gmail.com",
    "password": "Admin@123"
}
```

Expected response:

```text
User registered successfully
```

HTTP Status: `201 Created`

#### User Login

Endpoint:

`POST /api/auth/login`

Login workflow:

1. The client sends the registered email and password.
2. The service searches for the user using the email address.
3. The submitted password is verified against the stored BCrypt hash.
4. If authentication succeeds, the JWT service generates a token.
5. The token is returned to the client.

Example request:

```json
{
    "email": "admin@gmail.com",
    "password": "Admin@123"
}
```

Example response:

```json
{
    "token": "generated-jwt-token"
}
```

HTTP Status: `200 OK`

If the email or password is incorrect, authentication fails.

### Module 2: Patient Management

The patient management module provides CRUD functionality.

CRUD stands for:

- Create
- Read
- Update
- Delete

All patient endpoints require a valid JWT token.

#### 1. Add Patient

Endpoint:

`POST /api/patients`

Adds a new patient record to the database.

Example request:

```json
{
    "name": "Rahul Sharma",
    "age": 25,
    "gender": "Male",
    "disease": "Fever",
    "address": "Kolkata",
    "phoneNumber": "9876543210"
}
```

Expected response: Patient details with the generated ID.

HTTP Status: `201 Created`

#### 2. Get All Patients

Endpoint:

`GET /api/patients`

Retrieves all patient records stored in the database.

HTTP Status: `200 OK`

#### 3. Get Patient By ID

Endpoint:

`GET /api/patients/{id}`

Retrieves an individual patient using their unique database ID.

Example:

`GET /api/patients/1`

HTTP Status: `200 OK`

#### 4. Update Patient

Endpoint:

`PUT /api/patients/{id}`

Updates the details of an existing patient.

Example:

`PUT /api/patients/1`

Example request:

```json
{
    "name": "Rahul Sharma Updated",
    "age": 26,
    "gender": "Male",
    "disease": "Recovered",
    "address": "Kolkata",
    "phoneNumber": "9876543210"
}
```

HTTP Status: `200 OK`

#### 5. Delete Patient

Endpoint:

`DELETE /api/patients/{id}`

Deletes a patient record using its unique ID.

Example:

`DELETE /api/patients/1`

Expected response:

```text
Patient deleted successfully
```

HTTP Status: `200 OK`

## Database Design

The application uses MySQL as its relational database.

Database name:

`hospital_db`

The application contains two primary database tables.

### 1. Users Table

Stores registered user information.

| Column | Description |
|---|---|
| id | Primary key |
| name | User's full name |
| email | Unique user email |
| password | BCrypt-encrypted password |
| role | User role |

### 2. Patients Table

Stores patient information.

| Column | Description |
|---|---|
| id | Primary key |
| name | Patient's full name |
| age | Patient's age |
| gender | Patient's gender |
| disease | Patient's disease or condition |
| address | Patient's address |
| phone_number | Patient's phone number |

Hibernate automatically manages the database schema based on the entity definitions when schema update is enabled.

## JWT Authentication and Security

JSON Web Token (JWT) is used to secure patient management APIs.

The authentication process follows these steps:

1. A user registers with their credentials.
2. The user logs in using their registered email and password.
3. The application verifies the credentials.
4. The JWT service generates a signed token.
5. The client receives the token.
6. The client includes the token in the Authorization header for protected requests.
7. `JwtAuthenticationFilter` intercepts incoming requests.
8. The token is extracted and validated.
9. The user's identity and role are loaded.
10. Spring Security allows or rejects access based on authentication.

Authorization header format:

```text
Authorization: Bearer <JWT_TOKEN>
```

Security configuration:

- CSRF is disabled for the stateless REST API configuration.
- Authentication endpoints are publicly accessible.
- Patient management endpoints require authentication.
- Session management is stateless.
- Passwords are stored using BCrypt hashing.
- JWT tokens are signed and validated using a configured secret key.

## API Endpoint Summary

| HTTP Method | Endpoint | Description | Authentication |
|---|---|---|---|
| POST | `/api/auth/register` | Register user | Not Required |
| POST | `/api/auth/login` | Login user | Not Required |
| POST | `/api/patients` | Add patient | Required |
| GET | `/api/patients` | Get all patients | Required |
| GET | `/api/patients/{id}` | Get patient by ID | Required |
| PUT | `/api/patients/{id}` | Update patient | Required |
| DELETE | `/api/patients/{id}` | Delete patient | Required |

## Installation and Setup

### Prerequisites

Ensure the following software is installed:

- Java Development Kit (JDK 17 or compatible configured project version)
- Apache Maven
- MySQL Server
- IntelliJ IDEA or another Java IDE
- Postman

### Step 1: Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
```

Navigate to the project directory:

```bash
cd hospital-management-system
```

### Step 2: Create MySQL Database

Open MySQL Workbench or MySQL Command Line Client.

Execute:

```sql
CREATE DATABASE hospital_db;
```

### Step 3: Configure Database Connection

Open:

`src/main/resources/application.properties`

Configure the following properties:

```properties
spring.application.name=hospital-management-system

server.port=8080

spring.datasource.url=jdbc:mysql://localhost:3306/hospital_db
spring.datasource.username=root
spring.datasource.password=YOUR_MYSQL_PASSWORD
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

jwt.secret=YOUR_BASE64_ENCODED_SECRET
jwt.expiration=86400000
```

Replace `YOUR_MYSQL_PASSWORD` with your MySQL password.

Replace `YOUR_BASE64_ENCODED_SECRET` with a securely generated Base64-encoded JWT signing secret of sufficient length for the configured signing algorithm.

**Important:** Never commit real database passwords or JWT secrets to GitHub. Use environment variables or a local, untracked configuration file for sensitive values. The example values must be configured before running the application.

JWT expiration is configured in milliseconds. The value `86400000` represents 24 hours.

### Step 4: Build the Project

Run:

```bash
mvn clean install
```

This command:

- Cleans previous build files.
- Compiles the source code.
- Runs the configured tests.
- Packages the application.

### Step 5: Run the Application

Run using Maven:

```bash
mvn spring-boot:run
```

Alternatively, run `HospitalManagementSystemApplication.java` directly from IntelliJ IDEA.

The application starts at:

`http://localhost:8080`

## Testing

The application uses JUnit 5 and Mockito for unit testing.

The tests are organized according to the application layers.

### Service Layer Tests

#### AuthServiceTest

Tests:

- Successful user registration.
- Duplicate email registration.
- Successful login.
- Login with an unknown email.
- Login with an incorrect password.

#### PatientServiceTest

Tests:

- Adding a patient.
- Retrieving all patients.
- Retrieving a patient by ID.
- Retrieving a nonexistent patient.
- Updating patient details.
- Updating a nonexistent patient.
- Deleting a patient.
- Deleting a nonexistent patient.

### Controller Layer Tests

#### AuthControllerTest

Tests:

- User registration endpoint.
- User login endpoint.

#### PatientControllerTest

Tests:

- Add patient endpoint.
- Get all patients endpoint.
- Get patient by ID endpoint.
- Update patient endpoint.
- Delete patient endpoint.

### Security Tests

#### JwtServiceTest

Tests:

- JWT token generation.
- Extracting username from a token.
- Validating a valid token.
- Rejecting a token for the wrong email.
- Rejecting an invalid token.

#### JwtAuthenticationFilterTest

Tests:

- Request without a token.
- Request with a valid token.
- Request with an invalid token.
- Request with an unknown user.

### Running Tests

Run all tests:

```bash
mvn clean test
```

Run authentication service tests:

```bash
mvn -Dtest=AuthServiceTest test
```

Run patient service tests:

```bash
mvn -Dtest=PatientServiceTest test
```

Run controller tests:

```bash
mvn -Dtest="AuthControllerTest,PatientControllerTest" test
```

The service and controller unit tests use Mockito mocks and do not require database access. Repository tests, if included, use the configured MySQL database rather than an embedded H2 database.

## API Testing Using Postman

Postman is used to verify the REST APIs.

### Testing Workflow

1. Start the Spring Boot application.
2. Register a user using `/api/auth/register`.
3. Log in using `/api/auth/login`.
4. Copy the generated JWT token.
5. Open the Authorization tab for a patient API request.
6. Select `Bearer Token`.
7. Paste the generated JWT token.
8. Execute the patient CRUD requests.
9. Verify response bodies and HTTP status codes.

### Security Verification

Test the following cases:

- Access a patient endpoint without a token.
- Access a patient endpoint with an invalid token.
- Access a patient endpoint with a valid token.
- Verify that authentication endpoints remain publicly accessible.

Expected behavior:

- Requests with valid authentication can access protected patient APIs.
- Requests without a valid token are rejected by Spring Security.

## Application Workflow

```text
                 CLIENT / POSTMAN
                        |
                        |
                 AUTHENTICATION
                        |
               +--------+--------+
               |                 |
            REGISTER            LOGIN
               |                 |
               |           Verify Credentials
               |                 |
               |           Generate JWT
               |                 |
               +--------+--------+
                        |
                   JWT TOKEN
                        |
                        |
               PATIENT MANAGEMENT
                        |
               JWT Authentication
                        |
              +---------+---------+
              |         |         |
            CREATE    READ      UPDATE
              |         |         |
              +---------+---------+
                        |
                      DELETE
                        |
                        |
                  PATIENT SERVICE
                        |
                 PATIENT REPOSITORY
                        |
                      MYSQL
```

## Key Features

- RESTful API-based backend architecture.
- Secure user registration and login.
- BCrypt password hashing.
- JWT-based stateless authentication.
- Spring Security integration.
- Patient CRUD operations.
- MySQL database integration.
- Spring Data JPA and Hibernate ORM.
- DTO-based request and response handling.
- Input validation.
- Layered application architecture.
- Unit testing with JUnit 5 and Mockito.
- API verification using Postman.

## Future Enhancements

The following features can be considered for future development:

- Global exception handling with standardized error responses.
- Role-based authorization with different access permissions.
- Pagination and sorting for patient records.
- Search patients by name or disease.
- Centralized API documentation using Swagger/OpenAPI.
- Integration testing of complete HTTP and security workflows.
- Improved logging and monitoring.
- Docker-based application deployment.

## Conclusion

The Hospital Management System demonstrates the development of a secure backend application using Java and Spring Boot.

It combines user authentication, JWT-based API security, database integration, and patient CRUD operations using a layered architecture.

The project provides practical experience with REST API development, Spring Security, JWT authentication, Spring Data JPA, MySQL, unit testing, and API verification through Postman.

The application establishes a foundation for developing more advanced hospital management functionality in the future.

---

**Developed using Java, Spring Boot, Spring Security, JWT, Spring Data JPA, MySQL, Maven, JUnit 5, Mockito, and Postman.**