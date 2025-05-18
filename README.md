# Story Generator Backend

A Spring Boot backend service that handles authentication, email confirmation, video generation requests, and file management for the Story Generator project. This service works in tandem with a Flask-based microservice that processes the actual video generation as a part of Story Generator project.

## 🚀 Features

* **User Authentication** - Complete authentication flow with signup, email verification, and JWT-based login
* **Video Generation** - API endpoints to request short narrated videos with chosen background and voice
* **Asynchronous Processing** - Status checking for video generation jobs
* **Secure File Downloads** - Protected downloads of generated video files

## 📁 Project Structure

```
com.ndev.storyGeneratorBackend
├── controllers
│   ├── AuthController          # User authentication endpoints
│   ├── StoryGeneratorController # Video generation endpoints
│   └── TestController           # Testing endpoints
├── dtos
│   ├── SignupDTO               # User registration data transfer object
│   └── StoryRequestDTO         # Video generation request data transfer object
├── models
│   └── User                    # User entity class
├── repositories
│   └── UserRepository          # JPA repository for User entity
├── security
│   ├── AuthEntryPointJwt       # JWT authentication entry point
│   ├── AuthTokenFilter         # JWT token filter
│   ├── JwtUtil                 # JWT utility functions
│   └── WebSecurityConfig       # Spring Security configuration
├── services
│   ├── AuthService             # User authentication service
│   ├── CustomUserDetails       # Custom user details implementation
│   ├── CustomUserDetailsService # User details service implementation
│   ├── FileProcessingServiceImpl # File handling service
│   ├── FlaskApiService         # Service for communicating with Flask API
│   ├── StoryGenerationService  # Service for story generation logic
│   └── Utils                   # Utility functions
└── StoryGeneratorBackendApplication # Main application class
```

## 🔧 Installation

1. **Clone the repository**

```bash
git clone https://github.com/justndev/story-generator-backend.git
cd story-generator-backend
```

2. **Configure application properties**

Create or update `src/main/resources/application.properties`:

```properties
spring.application.name=storyGeneratorBackend
logging.level.org.springframework.security=DEBUG
spring.datasource.url: jdbc:h2:file:./data/demo-db
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
# Hibernate Configuration
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=update
# JWT Configuration
jwt.secret=672fc1b083d3d120cf51bc0b87a068d2247d313667821fd08a51eda699db60e6f18e17bef4f196a941980625904312aec61e40c37a29e7e3068aaf4f7cea154e24343e4431879cde501ea08bfa7fcb3acccd3bafaf623d59a0fbf0d499e8594291fa11fa02861ffe5e130747b2ed760bbd52276e364b5fbe38e34c813fb44a7ee6a444b308e61b48bd31ac52bffd43d171228b042dd4de82672993c297e60ff0616f1000e6248c94c34e30d60c001879ccaa0b4b0714393a343e2a29450dcf29a1b4dec8b0c2afa881ea7a4ce26e06bc49148bb1fb2c9af3de7040a7d8bbadc9b38e6d9723058e64924229c983e0b0ad58c3c81ca9c3cc0a403404240f622eaf
jwt.expiration=3600000
# SMTP Configrutation
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=
spring.mail.password=
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

3. **Build the project**

```bash
./mvnw clean install
```

4. **Run the application**

```bash
./mvnw spring-boot:run
```

## 🎯 API Endpoints

### Authentication

#### POST /api/auth/signup
Register a new user.

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securePassword",
  "firstName": "John",
  "lastName": "Doe"
}
```

**Response:**
```
success
```

#### GET /api/auth/verify?code={verificationCode}
Verify user email with the code sent to their email.

**Response:**
```
success
```

#### POST /api/auth/login
Authenticate a user and get JWT token.

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securePassword"
}
```

**Response:**
User object with JWT token.

#### GET /api/auth/check
Verify if a JWT token is valid.

**Response:**
```
success
```

### Story Generation

#### POST /api/story/generate
Request a new video generation.

**Request Body:**
```json
{
  "text": "Once upon a time...",
  "bgVideoId": "mc_parkour_1",
  "voice": "nova"
}
```

**Response:**
A JSON response indicating the job has been queued with a unique ID.

#### GET /api/story/status?uid={uniqueId}
Check the status of a video generation job.

**Response:**
```json
{
  "status": "completed",
  "uid": "unique-session-id"
}
```

#### GET /api/story/download?fileName={fileName}
Download a generated video file.

**Response:**
The video file as a downloadable resource.

## 🔄 Integration with Flask API

This Spring Boot application communicates with a separate Flask microservice that handles the actual video generation. The integration works as follows:

1. User makes a request to the Spring Boot backend with story text and preferences
2. Backend validates the request and forwards it to the Flask API
3. Flask API processes the request asynchronously
4. User can check the status through the Spring Boot backend
5. Once complete, the user can download the generated video from the Spring Boot backend

## 📦 Requirements

* Java 17+
* MySQL 8.0+
* Spring Boot 3.x
* The Flask API microservice must be running (see companion README)

## 🔒 Security

The application implements the following security features:

* JWT-based authentication
* Email verification for new users
* Path traversal protection for file downloads
* CORS configuration

## 🛡️ File Security Improvements

The file processing service includes protection against directory traversal attacks:

1. Path normalization
2. Canonical path verification
3. File existence validation

## 📝 License

MIT License
