# DogCare

DogCare is a full-stack web application for dog owners and veterinary clinics.

The platform provides pet profile management, veterinary appointments, medical records, vaccinations, analyses, clinic management, interactive maps, and AI-generated pet care recommendations.

The project is built with **React** on the frontend and **FastAPI + PostgreSQL** on the backend.

---

## Tech Stack

### Frontend

- React 18
- React Router
- Axios
- React Bootstrap
- Bootstrap 5
- Styled Components
- Framer Motion
- React Datepicker
- React Select
- Yandex Maps
- JWT Decode
- CRACO

### Backend

- Python
- FastAPI
- Uvicorn
- SQLAlchemy
- Pydantic
- Alembic
- PostgreSQL
- python-jose
- passlib
- bcrypt
- python-dotenv
- OpenAI Python SDK

### External Services

- OpenAI API
- Yandex Maps API

### Testing

- Pytest
- React Testing Library
- Postman

---

## Architecture

DogCare follows a standard three-tier architecture:

```text
┌─────────────────────────────┐
│          Frontend           │
│                             │
│   React + React Bootstrap   │
│   React Router              │
│   Axios                     │
│   Yandex Maps               │
└──────────────┬──────────────┘
               │
               │ HTTP / REST API
               │ JWT
               ▼
┌─────────────────────────────┐
│           Backend           │
│                             │
│   FastAPI                   │
│   Pydantic                  │
│   SQLAlchemy                │
│   Authentication            │
│   OpenAI Integration        │
└──────────────┬──────────────┘
               │
               │ SQLAlchemy ORM
               ▼
┌─────────────────────────────┐
│         PostgreSQL          │
│                             │
│   Users                     │
│   Pets                      │
│   Clinics                   │
│   Appointments              │
│   Vaccinations              │
│   Analyses                  │
│   Medicines                 │
└─────────────────────────────┘
```

The frontend communicates with the FastAPI backend through REST API requests.

The backend is responsible for authentication, business logic, database operations, pet medical data, appointments, clinic data, and AI recommendations.

---

## Main Features

### User Authentication

Users can register and log in to the application.

Authentication is implemented using JWT access tokens.

Two user roles are supported:

- pet owner
- veterinary clinic

---

### Pet Management

Pet owners can:

- add pets
- edit pet information
- delete pets
- manage multiple pets under one account
- store breed and age information
- receive personalized care recommendations

---

### Veterinary Appointments

Users can create and manage veterinary appointments.

Each appointment can contain:

- pet
- clinic
- date and time
- procedure
- current status
- conclusion status

The backend provides REST endpoints for creating, reading, updating, partially updating, and deleting appointments.

---

### Medical Records

The application supports several types of medical information:

- vaccinations
- vaccines
- medicines
- medicine intake
- medical analyses
- analysis types
- general veterinary appointments

This allows the application to maintain structured medical history for each pet.

---

### Veterinary Clinics

Veterinary clinics can be stored and managed through the backend.

The application supports:

- clinic creation
- clinic editing
- clinic deletion
- clinic selection
- clinic visualization on a map
- appointment management

Yandex Maps is used on the frontend for geographical visualization.

---

### AI Recommendations

DogCare integrates the OpenAI API to generate personalized pet-care recommendations.

Recommendations are generated using:

- pet breed
- pet age

The current backend sends this information to **GPT-4.1** and generates short recommendations related to pet care and nutrition.

---

## Project Setup

### 1. Clone the Repository

```bash
git clone https://github.com/medeuza/Kursovaya.git
cd Kursovaya
```

---

# Backend Setup

## 2. Create a Python Virtual Environment

From the backend project directory:

```bash
python3 -m venv .venv
```

Activate it.

### Linux / macOS

```bash
source .venv/bin/activate
```

### Windows

```bash
.venv\Scripts\activate
```

---

## 3. Install Python Dependencies

```bash
pip install -r requirements.txt
```

Main backend dependencies include:

```text
FastAPI
Uvicorn
SQLAlchemy
Alembic
Pydantic
PostgreSQL
OpenAI
python-jose
passlib
bcrypt
python-dotenv
```

---

## 4. Configure PostgreSQL

Make sure PostgreSQL is installed and running.

Create a database for the project:

```sql
CREATE DATABASE dogcare;
```

Configure the database connection used by:

```text
src/database.py
```

The backend uses SQLAlchemy ORM to communicate with PostgreSQL.

The application automatically initializes SQLAlchemy metadata when the FastAPI application starts.

---

## 5. Configure Environment Variables

Create a `.env` file in the backend directory.

The backend currently expects at least:

```env
OPENAI_KEY=your_openai_api_key
ORIGINS=http://localhost:3000
```

`ORIGINS` controls which frontend origins are allowed by FastAPI CORS middleware.

For example:

```env
ORIGINS=http://localhost:3000,http://127.0.0.1:3000
```

Also configure the PostgreSQL connection parameters required by `src/database.py`.

> Never commit `.env`, API keys, database passwords, or JWT secrets to GitHub.

---

## 6. Run the Backend

If the FastAPI application is stored in `main.py`:

```bash
uvicorn main:app --reload
```

Alternatively:

```bash
python -m uvicorn main:app --reload
```

By default, the API is available at:

```text
http://127.0.0.1:8000
```

Swagger documentation:

```text
http://127.0.0.1:8000/docs
```

ReDoc documentation:

```text
http://127.0.0.1:8000/redoc
```

---

# Frontend Setup

## 7. Install Node.js Dependencies

Open another terminal and go to the React client directory:

```bash
cd client
```

Install dependencies:

```bash
npm install
```

---

## 8. Start the React Application

```bash
npm start
```

The project uses **CRACO**, therefore this command internally runs:

```text
craco start
```

The frontend will normally be available at:

```text
http://localhost:3000
```

---

## Production Build

To build the frontend for production:

```bash
npm run build
```

The command runs:

```text
craco build
```

and creates an optimized production build.

---

# Running the Complete Project

The application requires three components:

```text
PostgreSQL
     +
FastAPI Backend
     +
React Frontend
```

A typical local development workflow is:

### Terminal 1 — PostgreSQL

Make sure PostgreSQL is running.

For Linux:

```bash
sudo systemctl start postgresql
```

---

### Terminal 2 — FastAPI

```bash
cd <backend-directory>

source .venv/bin/activate

uvicorn main:app --reload
```

Backend:

```text
http://localhost:8000
```

Swagger:

```text
http://localhost:8000/docs
```

---

### Terminal 3 — React

```bash
cd client

npm install
npm start
```

Frontend:

```text
http://localhost:3000
```

---

# API

The FastAPI backend provides endpoints for the main entities of the application.

## Authentication

```text
POST /users/register
POST /users/login
```

After successful login, the backend returns a Bearer JWT token.

Example response:

```json
{
  "access_token": "...",
  "token_type": "bearer"
}
```

---

## Pets

The backend supports retrieving and managing pet profiles.

Example:

```text
GET    /pets/
PUT    /pets/{id}
DELETE /pets/{id}
```

When authenticated, users can retrieve pets belonging to their account.

---

## Breeds

```text
POST   /breeds/
GET    /breeds/
PUT    /breeds/{id}
DELETE /breeds/{id}
```

---

## Veterinary Clinics

```text
POST   /clinics/
GET    /clinics/
PUT    /clinics/{id}
DELETE /clinics/{id}
```

---

## Appointments

```text
POST   /appointments/
GET    /appointments/
GET    /appointments/{id}
PUT    /appointments/{id}
PATCH  /appointments/{id}
DELETE /appointments/{id}
```

The `PATCH` endpoint can be used to update selected appointment properties such as status without replacing the complete appointment object.

---

## Vaccines

```text
POST   /vaccines/
GET    /vaccines/
PUT    /vaccines/{id}
DELETE /vaccines/{id}
```

---

## Vaccinations

```text
POST   /vaccinations/
GET    /vaccinations/
PUT    /vaccinations/{id}
DELETE /vaccinations/{id}
```

---

## Medicines

```text
POST   /medicines/
GET    /medicines/
PUT    /medicines/{id}
DELETE /medicines/{id}
```

---

## Medicine Intake

```text
POST   /medicine-takes/
GET    /medicine-takes/
PUT    /medicine-takes/{id}
DELETE /medicine-takes/{id}
```

---

## Medical Analyses

```text
POST /analysis-types/
GET  /analysis-types/

POST /analyses/
GET  /analyses/
```

---

## AI Recommendations

```text
POST /recommendations/
```

The endpoint receives information about the pet and requests a personalized recommendation from OpenAI.

Example concept:

```json
{
  "breed_id": 1,
  "age": 5
}
```

The backend retrieves the breed from PostgreSQL and passes the pet's age and breed to the OpenAI API.

---

# Database

The backend uses **SQLAlchemy ORM**.

Important database entities include:

```text
User
Pet
Breed
VeterinaryClinic
Appointment
Vaccine
Vaccination
Medicine
MedicineTake
AnalysisType
MedicalAnalysis
```

The application separates:

```text
models
schemas
repositories
authentication
database configuration
API routes
```

which keeps database operations separate from request validation and API logic.

---

# Authentication Flow

The authentication flow is:

```text
User
  │
  │ email + password
  ▼
POST /users/login
  │
  ▼
FastAPI
  │
  ├── verify credentials
  │
  └── create JWT
  ▼
Bearer Token
  │
  ▼
React Client
  │
  ▼
Authenticated API Requests
```

The JWT contains information about the authenticated user, including the user's email and role.

Passwords are handled using cryptographic password-hashing libraries rather than being stored as plain text.

---

# OpenAI Integration

The backend uses the official OpenAI Python SDK:

```python
from openai import OpenAI
```

The client is initialized using:

```env
OPENAI_KEY
```

Current recommendation generation uses:

```text
gpt-4.1
```

The model receives the pet's:

```text
breed
age
```

and generates a short recommendation about care and nutrition.

---

# Yandex Maps Integration

The React client uses:

```text
@pbe/react-yandex-maps
```

to display veterinary clinics geographically.

This allows clinic information to be combined with an interactive map directly in the application interface.

---

# Frontend Libraries

The frontend uses several libraries for UI and application behavior:

```text
React 18
React Router
Axios
Bootstrap
React Bootstrap
Bootstrap Icons
Styled Components
Framer Motion
React Datepicker
React Select
date-fns
JWT Decode
Yandex Maps
```

### Axios

Used for communication between React and the FastAPI REST API.

### React Router

Used for client-side navigation.

### React Bootstrap / Bootstrap

Used for interface components and responsive layout.

### React Datepicker

Used for selecting appointment dates.

### JWT Decode

Used on the client side to work with authentication tokens.

### Framer Motion

Used for UI animations.

---

# Backend Libraries

Important backend libraries include:

```text
fastapi
uvicorn
sqlalchemy
alembic
pydantic
openai
python-jose
passlib
bcrypt
python-dotenv
python-multipart
httpx
```

### FastAPI

REST API framework.

### SQLAlchemy

Database ORM.

### Alembic

Database migration management.

### Pydantic

Request and response validation.

### python-jose

JWT token handling.

### passlib + bcrypt

Password hashing.

### python-dotenv

Environment variable configuration.

### Uvicorn

ASGI server used to run FastAPI.

---

# Testing

## Frontend

Run React tests with:

```bash
npm test
```

The frontend includes:

```text
@testing-library/react
@testing-library/jest-dom
@testing-library/user-event
```

---

## Backend

Backend tests can be run with:

```bash
pytest
```

API requests can also be tested interactively through Swagger:

```text
http://localhost:8000/docs
```

or using Postman.

---

# Development Commands

### Frontend

```bash
npm start
```

Start the development server.

```bash
npm run build
```

Create a production build.

```bash
npm test
```

Run frontend tests.

---

### Backend

```bash
uvicorn main:app --reload
```

Start FastAPI in development mode.

```bash
pytest
```

Run backend tests.

---

# Development Workflow

```text
1. Start PostgreSQL
        ↓
2. Activate Python virtual environment
        ↓
3. Start FastAPI / Uvicorn
        ↓
4. Start React development server
        ↓
5. Open http://localhost:3000
        ↓
6. Use http://localhost:8000/docs for API debugging
```

---

## Environment

Recommended development setup:

```text
Python 3.10+
Node.js 18+
PostgreSQL
npm
```

---

## Author

**Ekaterina Nikitina**

HSE University  
Faculty of Computer Science
