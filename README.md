# Book Library Backend

A backend application built with **FastAPI, PostgreSQL, and SQLAlchemy**, featuring user authentication, role-based access control, book and rental management, and an AI-powered library assistant using Google's Gemini API.

The AI assistant can search the library's book database through function calling and stream generated responses to the client.

## Features

### Backend API
- RESTful API built with FastAPI.
- Asynchronous database operations using SQLAlchemy 2.0.
- PostgreSQL database integration.
- Organized architecture separating routers, schemas, services, and models.
- Database schema migrations using Alembic.

### Authentication and Authorization
- User registration and login.
- Password hashing.
- JWT-based authentication.
- Role-based access control with member and admin roles.
- Protected endpoints for administrative operations.
- User deactivation and role management.

### Library Management
- Create, retrieve, update, and delete books.
- Author management and author-book relationships.
- Book search by full or partial title.
- Book deactivation.
- Rental management.

### AI Library Assistant
- Integration with Google's Gemini API.
- Streaming AI responses.
- Function calling for database-backed book searches.
- Two-stage interaction: execute a database search when requested, then pass the result back to the model to generate a response.
- Primary and fallback model handling.
- Retry handling for selected API errors.
- Token usage and API cost calculation.

## Tech Stack

| Category | Technologies |
|---|---|
| Backend | Python, FastAPI |
| Database | PostgreSQL |
| ORM | SQLAlchemy 2.0 |
| Migrations | Alembic |
| Validation | Pydantic |
| Authentication | JWT, password hashing |
| AI Integration | Google Gemini API |
| HTTP Client | HTTPX |
| Testing | Pytest, Pytest-Asyncio |

## Project Structure

```text
book_library_backend_architecture/
│
├── authentication/
│   ├── dependencies.py
│   ├── jwt.py
│   └── pwd.py
│
├── config/
│   ├── ai_api.py
│   └── model_pricing.py
│
├── enums/
│   └── user_enums.py
│
├── models/
│   ├── authors.py
│   ├── books.py
│   ├── rental.py
│   └── users_model.py
│
├── prompts/
│   └── book_recommendation.py
│
├── routers/
│   ├── ai_router.py
│   ├── auth_router.py
│   ├── author_router.py
│   ├── books_router.py
│   └── rental_router.py
│
├── schemas/
│   ├── ai_schema.py
│   ├── author_schema.py
│   ├── book_schema.py
│   ├── rental_schema.py
│   └── user_schema.py
│
├── services/
│   ├── ai_cost.py
│   ├── ai_interaction.py
│   ├── ai_service.py
│   ├── author_service.py
│   ├── book_service.py
│   ├── rental_service.py
│   └── user_service.py
│
├── tests/
│   ├── ai_test.py
│   ├── conftest.py
│   ├── factory.py
│   ├── test_author.py
│   ├── test_book.py
│   ├── test_rental.py
│   └── test_user.py
│
├── tools/
│   ├── book_definitions.py
│   └── book_tools.py
│
├── alembic/
├── bootstrap.py
├── database.py
├── main.py
├── requirements.txt
└── .env.example
```

## Getting Started

### Prerequisites

- Python 3.12 or later.
- PostgreSQL.
- A Google Gemini API key.

### 1. Clone the repository

```bash
git clone https://github.com/kareem3228/book_library_backend_architecture.git

cd book_library_backend_architecture
```

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Activate it:

**Windows:**
```powershell
.venv\Scripts\activate
```

**Linux / macOS:**
```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure environment variables

Create a `.env` file in the project root:

```env
DATABASE_URL=postgresql+asyncpg://username:password@localhost:5432/book_library

SECRET_KEY=your_secret_key

GEMINI_API_KEY=your_gemini_api_key

OPEN_ROUTER_API_KEY=your_openrouter_api_key

TEST_DATABASE_URL=postgresql+asyncpg://username:password@localhost:5432/book_library_test
```

Replace the database credentials and API keys with your own values.

**Configuration note:** The current `config/ai_api.py` reads `GIMINI_API_KEY` rather than `GEMINI_API_KEY`. Standardize the variable name in the code before using the configuration above.

The test suite also requires a separate test database configured through `TEST_DATABASE_URL`.

### 5. Run database migrations

After configuring PostgreSQL and the database URL:

```bash
alembic upgrade head
```

### 6. Start the application

```bash
uvicorn main:app --reload
```

The API will be available at:

```text
http://127.0.0.1:8000
```

Interactive API documentation:

```text
http://127.0.0.1:8000/docs
```

## AI Assistant Workflow

The AI assistant uses Gemini function calling to retrieve information from the library database.

Its interaction flow:

1. The user submits a book-related preference or request.
2. The backend sends the request to the Gemini model using streaming.
3. If the model requests a book search, the backend collects the function-call arguments.
4. The backend executes the corresponding database search.
5. The search results are returned to Gemini using the function call ID and previous interaction ID.
6. Gemini generates the final response, which is streamed back to the client.

The implementation also includes retry handling, a fallback model, and token-based cost calculation.

## API Testing

The project uses Pytest and Pytest-Asyncio for asynchronous API testing.

The test suite covers areas including:

- User registration and login.
- Authentication and authorization.
- Admin and member permissions.
- Book CRUD operations.
- Author and rental operations.
- Invalid resource requests.
- AI interaction retries and fallback behavior.

Run the test suite:

```bash
pytest
```

Some AI integration tests make requests to the external Gemini API and therefore require valid API credentials and network access.

## Current Scope

This repository focuses on backend engineering and applied AI integration. It demonstrates the implementation of a structured API, database-backed operations, authentication, automated testing, and an LLM-powered feature.

The project is an ongoing development project.
