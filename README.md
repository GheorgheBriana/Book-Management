![Build](https://img.shields.io/badge/build-passing-brightgreen)

# Book Management 📚

## 1. Description

**BookManagement** is a web application built with Spring Boot that manages books, authors, genres, users, and reviews. It was developed in a team of two students as part of the **"Web Applications for Databases" (Master, Year 1, 2025)** course.

The application demonstrates the use of multiple entity relationships, CRUD operations, validation, authentication, logging, pagination, and testing, according to the project requirements.

---

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

---

## Entities and Relationships
The application defines **6 entities** with all relationship types:

- **User** <-> `@OneToMany` Reviews, `@OneToMany` UserBook  
- **Book** <-> `@ManyToOne` Author, `@ManyToOne` Genre, `@OneToMany` Reviews, `@OneToMany` UserBook  
- **Author** <-> `@OneToMany` Books  
- **Genre** <-> `@OneToMany` Books  
- **Review** <-> `@ManyToOne` User, `@ManyToOne` Book  
- **UserBook** <-> `@ManyToOne` User, `@ManyToOne` Book (join table for ManyToMany)  

| Entity | Relationships |
|--------|---------------|
| **User** | `@OneToMany` Reviews, `@OneToMany` UserBook |
| **Book** | `@ManyToOne` Author, `@ManyToOne` Genre, `@OneToMany` Reviews, `@OneToMany` UserBook |
| **Author** | `@OneToMany` Books |
| **Genre** | `@OneToMany` Books |
| **Review** | `@ManyToOne` User, `@ManyToOne` Book |
| **UserBook** | `@ManyToOne` User, `@ManyToOne` Book |
---

## 4. How to Run ⚙️

### 1. Configure the Database
Create the development database in MySQL:
```sql
CREATE DATABASE book_management;
```
Update database credentials in `src/main/resources/application-dev.yml`:
```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/book_management
    username: your_username
    password: your_password
```
### 2. Build and Run
```bash
mvn clean spring-boot:run -Dspring-boot.run.profiles=dev
```
### 3. Access the Application
- **Login page**: http://localhost:8080/users/login
- **Register page**: http://localhost:8080/users/register
- **Books list**: http://localhost:8080/books
- **Authors list**: http://localhost:8080/authors
- **Book reviews**: http://localhost:8080/reviews/book/{book_id}

---
## Architecture and Technologies Used

```mermaid
graph LR
    UI[Thymeleaf] --> C[Controller]
    C --> S[Service]
    S --> R[Repository]
    R --> DB[(MySQL/H2)]
    S --> Sec[Spring Security]
    Logs((Logback)) --> S

```

| Technology | Version |
|------------|---------|
| Java | 17 |
| Spring Boot | 3.x |
| MySQL | 8.0 |
| H2 Database | (for testing) |
| JUnit | 5 |
| Maven | |
| Lombok | |
| SLF4J | |
---

## Project Structure
```
BookManagement/
└── src/main/java/com/unibuc/bookmanagement
    ├── aspects/          # AOP classes (logging, performance monitoring)
    ├── config/           # Configuration classes
    ├── controllers/      # Web controllers
    ├── dto/              # Data Transfer Objects
    ├── errors/           # Error handling
    ├── exception/        # Custom exceptions
    ├── junction_tables/  # Many-to-many join tables
    ├── models/           # JPA entities
    ├── repositories/     # Data access layer
    ├── services/         # Business logic layer
    └── BookmanagementApplication.java  # Main application class
```
---

## Features 
- Users can register and log in securely.
- Authenticated users can browse books and authors.
- Admin users can add, edit, and delete books, authors, and genres.
- Users can write reviews for books and see reviews from others.
- Pagination and sorting available in books and authors listings.

## Security 
The application uses **Spring Security** with a custom `UserDetailsService` and JDBC-based authentication.

- **Password storage**: All user passwords are hashed using `BCryptPasswordEncoder`.  
- **Public routes**: `/users/register`, `/users/login`, and static resources (`/css/**`, `/js/**`, `/images/**`).  
- **Book browsing**: `/books`, `/books/{id}`, and `/books/find` are accessible without authentication.  
- **Admin-only routes**: Adding books or authors (`/books/add`, `/authors/add`) requires `ROLE_ADMIN`.  
- **Authenticated routes**: Any other `/books/**` endpoint (e.g., update, delete) and all other parts of the app require login.  
- **Login/Logout**: Custom login page at `/users/login`, with successful login redirecting to `/books`. Logout redirects to `/users/login?logout`.
---
## Demo 
**Live demo:** [http://localhost:8080/books](http://localhost:8080/books)

- **Register**  
  [http://localhost:8080/users/register](http://localhost:8080/users/register)
  
  ![Register](https://github.com/user-attachments/assets/743eac3d-b214-43bc-9bdf-937d83447676)

- **Login**  
  [http://localhost:8080/users/login](http://localhost:8080/users/login)
  After registration, the user is redirected to the login page where they must enter username and password.
  
  <img width="3027" height="1538" alt="image" src="https://github.com/user-attachments/assets/4943bebe-eef1-4b8b-9c56-7a26c07a8d05" />


- **Home Page**  
  [http://localhost:8080/books](http://localhost:8080/books)
  
  ![Home Page](https://github.com/user-attachments/assets/61f973c4-3e61-4db2-af4c-6870bfec3f43)

- **Book Details**  
  [http://localhost:8080/books/{id}](http://localhost:8080/books/{id})  
  Clicking on a book redirects to its details page, where users can add a review. Admins can delete the book from this page.
  
  ![Book Details](https://github.com/user-attachments/assets/05231a49-53bb-4753-b664-2abac2ff36c8)

- **Book Reviews**  
  [http://localhost:8080/reviews/book/{bookId}](http://localhost:8080/reviews/book/{bookId})  
  Displays all reviews for a specific book.
  
  ![Book Reviews](https://github.com/user-attachments/assets/92a80cb3-33f0-43d4-bcde-fc177890aac3)

- **Add Review**  
  A logged-in user can add a review by providing a description and a rating between 1 and 5.
  
  ![Add Review](https://github.com/user-attachments/assets/b1935335-f189-4725-86a9-d71d0b3e5f01)

- **Add Book (Admin only)**  
  Only admins can add books. The admin provides the title, description, and ISBN, then selects an existing author from the database (or adds a new one). After submission, the book is visible on `/books`.
  
  ![Add Book](https://github.com/user-attachments/assets/d4acce77-35f1-429e-a0cb-b92075102922)

- **Add Author**  
  ![Add Author](https://github.com/user-attachments/assets/4e5fb9a5-b309-4cc1-9a02-ef9d43628044)

- **Authors List**  
  ![Authors List](https://github.com/user-attachments/assets/6607d7d3-d76e-441e-b676-488439aab3cd)

---
## API Reference

### Books
| Method | Endpoint        | Description                        |
|--------|-----------------|------------------------------------|
| GET    | /books          | List all books (paginated)         |
| GET    | /books/{id}     | View details of a specific book    |
| GET    | /books/add      | Show add book form (HTML)          |
| POST   | /books/add      | Create a new book (requires ADMIN) |

### Authors
| Method | Endpoint            | Description                        |
|--------|---------------------|------------------------------------|
| GET    | /authors            | List all authors (paginated)       |
| GET    | /authors/add        | Show add author form (HTML)        |
| POST   | /authors/add        | Create a new author (requires ADMIN) |
| GET    | /authors/edit/{id}  | Show edit author form              |
| POST   | /authors/update/{id}| Update author details              |

### Reviews
| Method | Endpoint                  | Description                            |
|--------|---------------------------|----------------------------------------|
| GET    | /reviews/book/{bookId}    | List all reviews for a book            |
| GET    | /reviews/add/{bookId}     | Show add review form (HTML)            |
| POST   | /reviews/add/{bookId}     | Submit a new review for a book         |

### Genres (REST API)
| Method | Endpoint       | Description              | Response Example |
|--------|----------------|--------------------------|------------------|
| GET    | /api/genres    | List all genres          | `[{"id":1,"name":"Fiction"}]` |
| GET    | /api/genres/{id} | Get details of a genre | `{"id":1,"name":"Fiction"}`   |

### UserBooks (REST API)
| Method | Endpoint                       | Description                            | Response Example |
|--------|--------------------------------|----------------------------------------|------------------|
| GET    | /api/userbooks/users/{userId}  | List all books of a user               | `[{"userId":1,"bookId":2}]` |
| POST   | /api/userbooks                 | Create a user–book relation (JSON)     | `{"userId":1,"bookId":2}`   |

### Users
| Method | Endpoint        | Description             |
|--------|-----------------|-------------------------|
| GET    | /users/login    | Show login form (HTML)  |
| POST   | /users/login    | Authenticate user       |
| GET    | /users/register | Show register form      |
| POST   | /users/register | Create new user account |
---
## 5. Testing 
    Run all tests with H2 in-memory DB:

```bash
mvn test
```

### Test types:
  - Unit tests: services & repositories
  - Integration tests: controllers
  - Profile-specific config: test profile auto-configures H2
  
### Useful Test Commands

```bash
# Run a specific test class
mvn -Dtest=BookServiceTest test
# Run with detailed logs
mvn -Dspring-boot.run.profiles=test -Dlogging.level.root=DEBUG test
```
## Example Test 🧪

### AuthorService - Create Author
```java
@Test
void testCreateAuthor() {
    AuthorDTO dto = new AuthorDTO();
    dto.setName("Andrei Popescu");
    dto.setBirthDate("2000-01-01");

    Author savedAuthor = new Author();
    savedAuthor.setName("Andrei Popescu");

    when(authorRepository.save(any(Author.class))).thenReturn(savedAuthor);

    Author result = authorService.createAuthor(dto);

    assertNotNull(result);
    assertEquals("Andrei Popescu", result.getName());
    verify(authorRepository, times(1)).save(any(Author.class));
}
```
<img width="2521" height="1679" alt="image" src="https://github.com/user-attachments/assets/0140ed1a-8227-4beb-b5fc-db24f2fe3eb9" />

---

## Team 
- Student 1: Gheorghe Briana
- Student 2: Turcitu Ana  
- Course: Master’s in Databases and Software Technologies (Year 1, 2025)  
- Discipline: Web Applications for Databases  

## License 📄
This project was created for academic purposes. All rights reserved.
