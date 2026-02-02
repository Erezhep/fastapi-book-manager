# 📚 Project: BookTracker API

## 1. Project Overview
The **BookTracker API** is a RESTful service built with FastAPI that allows users to manage a personal library. Users can track books they’ve read, manage a "wishlist," and leave reviews. 

The goal of this project is to practice **relational database design**, **JWT authentication**, and **CRUD operations** using modern Python asynchronous patterns.

---

## 2. Technical Stack
* **Language:** Python 3.10+
* **Framework:** FastAPI
* **Database:** SQLite (Development) / PostgreSQL (Production)
* **ORM:** SQLAlchemy (using `AsyncSession`)
* **Authentication:** OAuth2 with JWT (JSON Web Tokens)
* **Data Validation:** Pydantic v2

---

## 3. Database Schema
The system requires a relational structure to link users to their specific book collections.



### Entities:
* **User:** `id`, `username`, `email`, `hashed_password`, `is_active`.
* **Book:** `id`, `title`, `author`, `isbn`, `genre`, `publication_year`.
* **UserBook (Association Table):** `user_id`, `book_id`, `status` (Reading, Completed, Wishlist), `rating` (1-5), `review_text`, `date_added`.

---

## 4. Functional Requirements

### Phase 1: User Management & Security
* **Registration:** Endpoint to create an account with a unique email and hashed password using `passlib`.
* **Authentication:** Implement the OAuth2 password flow to exchange credentials for a JWT.
* **Authorization:** Create a dependency to extract the current user from the token to protect private routes.

### Phase 2: The Global Catalog
* **Read:** Public access to list all books in the database.
* **Search/Filter:** Support query parameters for `?genre=fiction` or `?author=Orwell`.
* **Admin Control:** Restrict `POST`, `PUT`, and `DELETE` operations on the global book list to users with an `admin` flag.

### Phase 3: Personal Library Tracking
* **Collection Management:** Users can add books to their personal "shelf."
* **Progress Tracking:** Users can update their reading status (e.g., moving a book from "Wishlist" to "Completed").
* **Reviews:** Users can leave a numerical rating and a short text review for books they have read.

---

## 5. API Endpoints

| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `POST` | `/auth/register` | Register a new account | No |
| `POST` | `/auth/token` | Login and receive JWT access token | No |
| `GET` | `/books` | List books with pagination & filters | No |
| `POST` | `/books` | Add a new book to the global catalog | Yes (Admin) |
| `GET` | `/my-library` | View your personal collection | Yes |
| `POST` | `/my-library/{book_id}` | Add a specific book to your shelf | Yes |
| `PATCH` | `/my-library/{book_id}` | Update status or rating | Yes |
| `DELETE` | `/my-library/{book_id}` | Remove book from your shelf | Yes |

---

## 6. Development Milestones

1.  **Project Scaffolding:** Set up `main.py`, `models.py`, `schemas.py`, and `database.py`.
2.  **Database Connection:** Configure SQLAlchemy and create an async engine.
3.  **Authentication Layer:** Implement password hashing and token generation.
4.  **CRUD Implementation:** Build out the book catalog and user-library logic.
5.  **Error Handling:** Implement custom exception handlers for `404 Not Found` and `403 Forbidden`.
6.  **Testing:** Use the `/docs` Swagger UI to verify that a user cannot edit another user's library.

---

## 7. Success Criteria
* [ ] API passes all Pydantic validation checks.
* [ ] JWT tokens expire after a set time (e.g., 30 minutes).
* [ ] Database relationships properly handle cascading deletes (if a book is deleted, the user-link is removed).
* [ ] Code is modular (separated into routers, services, and models).