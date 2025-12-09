# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Spring Boot RESTful web service demo project based on the Spring Getting Started guide. It implements a simple greeting service that accepts HTTP GET requests and returns JSON responses.

**Main Endpoint**: `/greeting` - Returns a greeting message with an auto-incrementing ID
- Optional query parameter: `name` (defaults to "World")
- Response format: `{"id":1,"content":"Hello, World!"}`

## Architecture

The project follows a standard Spring Boot REST architecture:

- **RestServiceApplication** - Main Spring Boot application entry point
- **GreetingController** (`@RestController`) - Handles `/greeting` endpoint with `@GetMapping`
- **Greeting** - Java record representing the JSON response model
- Uses AtomicLong for thread-safe counter incrementation across requests
- Jackson for automatic JSON serialization (included via spring-boot-starter-webmvc)

## Build and Development Commands

**Important**: All Maven commands must be run from the `complete/` directory, as the actual project structure resides there.

### Building and Testing

```bash
# Run from complete/ directory
cd complete

# Clean and run tests
mvn clean test

# Build without running tests
mvn clean package -DskipTests=true

# Build with tests
mvn clean package -DskipTests=false

# Run the application locally
mvn spring-boot:run
# Access at: http://localhost:8080/greeting
```

### Testing the Service

Once running, test with:
```bash
curl http://localhost:8080/greeting
curl http://localhost:8080/greeting?name=User
```

### Docker

```bash
# Build Docker image (run from repository root)
docker build -t gs-rest-service complete/

# Run container
docker run -p 8080:8080 gs-rest-service
```

The Dockerfile uses multi-stage build:
1. Build stage: Maven with JDK 17 for compilation
2. Runtime stage: JRE 17 for minimal image size

## Testing Framework

The project uses Spring Boot 4.0.0 with RestTestClient for integration testing:
- `@SpringBootTest` - Loads full application context
- `@AutoConfigureRestTestClient` - Configures RestTestClient
- Tests validate both default and parameterized responses

## CI/CD

GitHub Actions workflow (`.github/workflows/build-and-push.yml`):
1. **Test job**: Runs Maven tests with caching
2. **Docker job**: Builds and pushes image to Docker Hub (requires secrets: DOCKERHUB_USERNAME, DOCKERHUB_TOKEN)

## Configuration

### POM Properties
- Java version: 17
- Spring Boot: 4.0.0
- Sonar properties configured via environment variables:
  - `SONAR_PROJECT_KEY`
  - `SONAR_HOST_URL`
  - `SONAR_ORG`

### Dependencies
- `spring-boot-starter-webmvc` - Web MVC framework
- `spring-boot-starter-webmvc-test` - Testing support (test scope)

## Project Structure Note

The repository has an unusual structure where the actual project is in the `complete/` subdirectory. This is typical for Spring Getting Started guides which often include both "initial" and "complete" versions. When working with this project, always ensure you're in the `complete/` directory for build operations.
