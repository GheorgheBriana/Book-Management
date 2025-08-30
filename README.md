![Build](https://img.shields.io/badge/build-passing-brightgreen)

# Book Management 📚

## 1. Description

**BookManagement** is a web application built with Spring Boot that manages books, authors, genres, users, and reviews. It was developed in a team of two students as part of the **"Web Applications for Databases" (Master, Year 1, 2025)** course.

The application demonstrates the use of multiple entity relationships, CRUD operations, validation, authentication, logging, pagination, and testing, according to the project requirements.


## Covered Requirements
1. **Entity relationships**: Includes `@OneToOne`, `@OneToMany`, `@ManyToOne`, `@ManyToMany`.  
2. **CRUD operations**: Full Create, Read, Update, Delete implemented for all entities via controllers and views.  
3. **Profiles & databases**:  
   - `dev` profile -> uses **MySQL** for development/production.  
   - `test` profile -> uses **H2 in-memory** database for testing.  
4. **Testing**: Both unit and integration tests using JUnit and Spring Boot Test.  
5. **Views & validation**: Thymeleaf forms with `@Valid`, custom exception handling (`@ControllerAdvice`).  
6. **Logging**: Configured via SLF4J (application + persistence layer logs).  
7. **Pagination & sorting**: Implemented for listing entities with Spring Data `Pageable`.  
8. **Spring Security**: JDBC authentication with `users` and `roles` tables.  

## Entities and Relationships
The application defines at least **6–7 entities** with all relationship types:

- **User** <-> `@OneToMany` Reviews, `@OneToMany` UserBook  
- **Book** <-> `@ManyToOne` Author, `@ManyToOne` Genre, `@OneToMany` Reviews, `@OneToMany` UserBook  
- **Author** <-> `@OneToMany` Books  
- **Genre** <-> `@OneToMany` Books  
- **Review** <-> `@ManyToOne` User, `@ManyToOne` Book  
- **UserBook** <-> `@ManyToOne` User, `@ManyToOne` Book (join table for ManyToMany)  

## 4. How to Run ⚙️

### 4.1 Configure the Database
- Create the development database in MySQL:
```sql
CREATE DATABASE book_management 

    Update src/main/resources/application-dev.yml with your MySQL username and password.

4.2 Build and Run
    mvn clean spring-boot:run -Dspring-boot.run.profiles=dev

4.3 Access in Browser
    Login page -> http://localhost:8080/users/login
    Register page -> http://localhost:8080/users/register
    Books list -> http://localhost:8080/books
    Authors list -> http://localhost:8080/authors
    Reviews for a specific book-> -> http://localhost:8080/reviews/book/book_id

## 5. Testing 
- Run all tests with H2 in-memory DB:
    mvn test

- Typical test types included:
    • Unit tests: services & repositories
    • Integration tests: controllers
    • Profile-specific config: test profile auto-configures H2

- Useful tips:
    • To run a single test class:
        mvn -Dtest=SomeTestClass test
    • To run with detailed logs:
        mvn -Dspring-boot.run.profiles=test -Dlogging.level.root=DEBUG test

## 6. Technologies 
- Java 17
- Spring Boot 3 (Web, Data JPA, Validation, Security, Thymeleaf)
- MySQL (dev), H2 (test)
- JUnit 5 & Spring Boot Test
- Maven
- Lombok
- SLF4J logging
  
## 7. Project Structure 📂
BookManagement/
 ┣ src/main/java/com/unibuc/bookmanagement
 ┃ ┣ aspects/         # Contains AOP-related classes (e.g., logging, performance monitoring)
 ┃ ┣ config/          # Configuration classes (security, database, application profiles)
 ┃ ┣ controllers/     # Web controllers handling HTTP requests and responses
 ┃ ┣ dto/             # Data Transfer Objects used to pass data between layers
 ┃ ┣ errors/          # Centralized error handling logic
 ┃ ┣ exception/       # Custom exception classes
 ┃ ┣ junction_tables/ # Entities representing join tables for many-to-many relationships
 ┃ ┣ models/          # JPA entities (User, Book, Author, Genre, Review, UserBook, etc.)
 ┃ ┣ repositories/    # Spring Data JPA repositories for database access
 ┃ ┣ services/        # Service layer with business logic
 ┃ ┗ BookmanagementApplication.java  # Main entry point of the Spring Boot application
 ┣ src/main/resources
 ┃ ┣ templates/                # Thymeleaf templates (views for forms, lists, etc.)
 ┃ ┣ application.properties    # Default configuration
 ┃ ┣ application-dev.properties # Development profile (MySQL)
 ┃ ┗ application-test.properties # Test profile (H2 in-memory)
 ┣ pom.xml
 ┗ README.md



