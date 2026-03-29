# 🚗 Best Cars — Full-Stack Dealership Review Platform

![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-3.2-092E20?logo=django&logoColor=white)
![React](https://img.shields.io/badge/React-18.2-61DAFB?logo=react&logoColor=black)
![Node.js](https://img.shields.io/badge/Node.js-Express-339933?logo=node.js&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47A248?logo=mongodb&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Deployment-326CE5?logo=kubernetes&logoColor=white)
![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-2088FF?logo=github-actions&logoColor=white)

A full-stack web application that allows users to browse car dealerships across the United States, read and submit reviews, and receive AI-powered sentiment analysis on those reviews. Built as a capstone project for the IBM Full-Stack Developer Professional Certificate, this project demonstrates a **microservices architecture** combining Django, React, Node.js, MongoDB, Flask, Docker, and Kubernetes.

---

## 📋 Table of Contents

- [Features](#-features)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Local Development Setup](#local-development-setup)
  - [Docker Setup](#docker-setup)
- [API Reference](#-api-reference)
  - [Django REST API](#django-rest-api)
  - [Node.js Dealership API](#nodejs-dealership-api)
  - [Sentiment Analyzer API](#sentiment-analyzer-api)
- [Frontend Pages & Components](#-frontend-pages--components)
- [Database Schemas](#-database-schemas)
- [Deployment](#-deployment)
  - [Docker Compose](#docker-compose)
  - [Kubernetes](#kubernetes)
- [CI/CD Pipeline](#-cicd-pipeline)
- [Skills & Technologies Showcased](#-skills--technologies-showcased)

---

## ✨ Features

- 🔐 **User Authentication** — Secure registration, login, and logout using Django's built-in authentication system
- 🏪 **Dealership Browser** — Browse all US dealerships or filter by state
- 📝 **Review System** — Authenticated users can submit reviews for any dealership
- 🤖 **AI Sentiment Analysis** — Every review is automatically scored as *Positive*, *Neutral*, or *Negative* using NLP (VADER/NLTK)
- 🚘 **Car Inventory** — Manage car makes and models linked to specific dealerships
- 📡 **Microservices Architecture** — Decoupled services for dealerships, reviews, and sentiment analysis
- 🐳 **Containerized** — Full Docker and Docker Compose support
- ☸️ **Kubernetes-Ready** — Kubernetes deployment manifest included
- ⚡ **CI/CD** — Automated linting pipeline via GitHub Actions

---

## 🏗️ Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                        User (Browser)                        │
└───────────────────────────┬──────────────────────────────────┘
                            │ HTTP
                            ▼
┌──────────────────────────────────────────────────────────────┐
│              React Frontend (Port 3000 / Served by Django)   │
│    Login | Register | Dealers | Dealer Detail | Post Review  │
└───────────────────────────┬──────────────────────────────────┘
                            │ REST API calls
                            ▼
┌──────────────────────────────────────────────────────────────┐
│              Django Backend (Port 8000 / Gunicorn)           │
│   Auth | Car Models | Proxy to Node API | Proxy to Sentiment │
│                     SQLite3 Database                         │
└────────┬──────────────────────────────────────┬─────────────┘
         │ HTTP                                  │ HTTP
         ▼                                       ▼
┌─────────────────────┐             ┌────────────────────────┐
│ Node.js/Express API │             │ Flask Sentiment Service │
│     (Port 3030)     │             │      (Port 5050)        │
│  Dealership CRUD    │             │  VADER NLP Analysis     │
│  Review CRUD        │             │  positive/neutral/      │
│                     │             │  negative               │
└────────┬────────────┘             └────────────────────────┘
         │ Mongoose ODM
         ▼
┌─────────────────────┐
│   MongoDB Database  │
│    (Port 27017)     │
│  Dealerships        │
│  Reviews            │
└─────────────────────┘
```

---

## 🛠️ Tech Stack

### Backend

| Technology | Version | Role |
|------------|---------|------|
| **Python** | 3.12 | Primary backend language |
| **Django** | 3.2.5 | Main web framework, ORM, authentication |
| **Gunicorn** | latest | WSGI HTTP server (3 workers) |
| **Node.js** | latest | Dealership/Review microservice runtime |
| **Express.js** | ^4.18.2 | RESTful API framework for Node microservice |
| **Flask** | latest | Sentiment analysis microservice framework |
| **python-dotenv** | latest | Environment variable management |
| **Pillow** | latest | Image processing for Django |
| **requests** | latest | HTTP client for inter-service communication |

### Database

| Technology | Version | Role |
|------------|---------|------|
| **MongoDB** | latest | NoSQL document store for dealerships & reviews |
| **Mongoose** | ^8.0.1 | MongoDB ODM for Node.js |
| **SQLite3** | built-in | Relational database for Django models (cars, users) |

### Frontend

| Technology | Version | Role |
|------------|---------|------|
| **React** | ^18.2.0 | UI component library |
| **React Router** | ^6.19.0 | Client-side routing (SPA navigation) |
| **react-scripts** | 5.0.1 | Build toolchain (Create React App) |

### AI / NLP

| Technology | Role |
|------------|------|
| **NLTK** | Natural language toolkit |
| **VADER** | Valence Aware Dictionary and sEntiment Reasoner — rule-based sentiment analysis |

### DevOps & Infrastructure

| Technology | Role |
|------------|------|
| **Docker** | Container runtime for all services |
| **Docker Compose** | Multi-container orchestration (MongoDB + Node API) |
| **Kubernetes** | Production-grade container orchestration & deployment |
| **GitHub Actions** | CI/CD automation — linting on push/PR |

### Code Quality

| Technology | Role |
|------------|------|
| **flake8** | Python linter (PEP 8 compliance) |
| **JSHint** | JavaScript linter for Node.js service |

---

## 📁 Project Structure

```
xrwvm-fullstack_developer_capstone/
├── .github/
│   └── workflows/
│       └── main.yml             # GitHub Actions CI/CD pipeline
├── server/
│   ├── djangoapp/               # Django application
│   │   ├── models.py            # CarMake, CarModel ORM models
│   │   ├── views.py             # API view functions
│   │   ├── urls.py              # Django URL routing
│   │   ├── restapis.py          # Helper functions (calls Node API & Sentiment service)
│   │   ├── admin.py             # Django admin configuration
│   │   ├── populate.py          # Seed data for car makes & models
│   │   ├── .env                 # Environment variables (backend & sentiment URLs)
│   │   └── microservices/
│   │       └── app.py           # Flask sentiment analyzer microservice
│   ├── djangoproj/
│   │   ├── settings.py          # Django project settings
│   │   └── urls.py              # Root URL configuration
│   ├── frontend/                # React single-page application
│   │   ├── public/
│   │   ├── src/
│   │   │   ├── App.js           # Root component & route definitions
│   │   │   ├── index.js         # React entry point
│   │   │   └── components/
│   │   │       ├── Header/
│   │   │       │   └── Header.jsx
│   │   │       ├── Login/
│   │   │       │   └── Login.jsx
│   │   │       ├── Register/
│   │   │       │   └── Register.jsx
│   │   │       └── Dealers/
│   │   │           ├── Dealers.jsx      # Dealership listing page
│   │   │           ├── Dealer.jsx       # Single dealership + reviews
│   │   │           └── PostReview.jsx   # Review submission form
│   │   └── package.json
│   ├── database/                # Node.js / MongoDB microservice
│   │   ├── app.js               # Express server entry point
│   │   ├── dealership.js        # Mongoose Dealership schema & model
│   │   ├── review.js            # Mongoose Review schema & model
│   │   ├── Inventory.js         # Mongoose Inventory schema & model
│   │   ├── docker-compose.yml   # MongoDB + Node API Docker Compose
│   │   └── package.json
│   ├── Dockerfile               # Django app Docker image
│   ├── entrypoint.sh            # Docker entrypoint (migrate + collectstatic)
│   ├── manage.py                # Django management CLI
│   ├── requirements.txt         # Python dependencies
│   ├── deployment.yaml          # Kubernetes deployment manifest
│   └── package.json             # Root Node package config
├── LICENSE
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.12+
- Node.js 18+ & npm
- Docker & Docker Compose
- MongoDB (or use the Docker Compose setup)

### Local Development Setup

#### 1. Clone the Repository

```bash
git clone https://github.com/rafael-a-g-n/xrwvm-fullstack_developer_capstone.git
cd xrwvm-fullstack_developer_capstone
```

#### 2. Start the MongoDB + Node.js Microservice

```bash
cd server/database
docker-compose up --build -d
```

This starts:
- **MongoDB** on port `27017`
- **Node.js API** on port `3030`

#### 3. Set Up the Python Environment

```bash
cd server
pip install -r requirements.txt
```

#### 4. Configure Environment Variables

Create or update `server/djangoapp/.env`:

```env
backend_url=http://localhost:3030
sentiment_analyzer_url=http://localhost:5050/
```

#### 5. Start the Sentiment Analyzer Microservice

```bash
cd server/djangoapp/microservices
pip install flask nltk
python app.py
```

#### 6. Run Django Migrations & Start the Server

```bash
cd server
python manage.py makemigrations
python manage.py migrate
python manage.py runserver
```

#### 7. Build and Serve the React Frontend

```bash
cd server/frontend
npm install
npm run build
```

> The Django server serves the React build at `http://localhost:8000/`

---

### Docker Setup

Build and run the Django application in Docker:

```bash
cd server
docker build -t bestcars-django .
docker run -p 8000:8000 bestcars-django
```

For the full stack (MongoDB + Node API):

```bash
cd server/database
docker-compose up --build
```

---

## 📡 API Reference

### Django REST API

Base URL: `http://localhost:8000/djangoapp`

| Endpoint | Method | Auth Required | Description |
|----------|--------|:-------------:|-------------|
| `/login` | POST | ❌ | Authenticate user |
| `/logout` | GET | ✅ | Log out current user |
| `/register` | POST | ❌ | Register a new user |
| `/get_cars` | GET | ❌ | List all car makes & models |
| `/get_dealers` | GET | ❌ | List all dealerships |
| `/get_dealers/<state>` | GET | ❌ | List dealerships filtered by state |
| `/dealer/<dealer_id>` | GET | ❌ | Get details for a specific dealership |
| `/reviews/dealer/<dealer_id>` | GET | ❌ | Get all reviews for a dealership |
| `/add_review` | POST | ✅ | Submit a new review |

#### Example: Register a User

```http
POST /djangoapp/register
Content-Type: application/json

{
  "username": "johndoe",
  "password": "securepassword123",
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com"
}
```

#### Example: Post a Review

```http
POST /djangoapp/add_review
Content-Type: application/json

{
  "name": "John Doe",
  "dealership": 15,
  "review": "Excellent service and great prices!",
  "purchase": true,
  "purchase_date": "2023-10-01",
  "car_make": "Toyota",
  "car_model": "Camry",
  "car_year": 2022
}
```

---

### Node.js Dealership API

Base URL: `http://localhost:3030`

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Health check / welcome message |
| `/fetchDealers` | GET | Fetch all dealerships |
| `/fetchDealers/:state` | GET | Fetch dealerships by state code |
| `/fetchDealer/:id` | GET | Fetch a single dealership by ID |
| `/fetchReviews` | GET | Fetch all reviews |
| `/fetchReviews/dealer/:id` | GET | Fetch reviews for a specific dealer |
| `/insert_review` | POST | Insert a new review into MongoDB |

#### Example: Insert Review

```http
POST /insert_review
Content-Type: application/json

{
  "id": 101,
  "name": "John Doe",
  "dealership": 15,
  "review": "Great experience!",
  "purchase": true,
  "purchase_date": "2023-10-01",
  "car_make": "Toyota",
  "car_model": "Camry",
  "car_year": 2022
}
```

---

### Sentiment Analyzer API

Base URL: `http://localhost:5050`

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Health check |
| `/analyze/<input_text>` | GET | Analyze sentiment of provided text |

**Response values**: `positive` | `neutral` | `negative`

#### Example

```http
GET /analyze/The%20service%20was%20absolutely%20fantastic!
```

```json
"positive"
```

---

## 🖥️ Frontend Pages & Components

| Route | Component | Description |
|-------|-----------|-------------|
| `/login` | `Login.jsx` | User login form with Django session authentication |
| `/register` | `Register.jsx` | New user registration form |
| `/dealers` | `Dealers.jsx` | Browse all dealerships; filter by US state |
| `/dealer/:id` | `Dealer.jsx` | Dealership details, address, and all customer reviews with sentiment badges |
| `/postreview/:id` | `PostReview.jsx` | Authenticated form to submit a review for a dealership |

All pages share the `Header.jsx` component for navigation and authentication status display.

---

## 🗄️ Database Schemas

### Django / SQLite3

#### CarMake

| Field | Type | Description |
|-------|------|-------------|
| `id` | AutoField | Primary key |
| `name` | CharField(100) | Brand name (e.g., Toyota, Ford) |
| `description` | TextField | Brand description |
| `country` | CharField(100) | Country of origin (optional) |
| `website` | URLField | Brand website (optional) |

#### CarModel

| Field | Type | Description |
|-------|------|-------------|
| `id` | AutoField | Primary key |
| `car_make` | ForeignKey → CarMake | Associated car brand |
| `dealer_id` | IntegerField | Associated dealership (references MongoDB ID) |
| `name` | CharField(100) | Model name (e.g., Camry, F-150) |
| `type` | CharField | Body type: `SEDAN`, `SUV`, or `WAGON` |
| `year` | IntegerField | Model year (2015–2023) |

---

### MongoDB (Mongoose)

#### Dealership

```javascript
{
  id:         Number,   // Unique dealership ID
  city:       String,   // City name
  state:      String,   // State abbreviation (e.g., "TX")
  address:    String,   // Street address
  zip:        String,   // ZIP code
  lat:        String,   // Latitude
  long:       String,   // Longitude
  short_name: String,   // Abbreviated dealership name
  full_name:  String    // Full dealership name
}
```

#### Review

```javascript
{
  id:            Number,   // Unique review ID
  name:          String,   // Reviewer's name
  dealership:    Number,   // Foreign key → Dealership.id
  review:        String,   // Review text (analyzed for sentiment)
  purchase:      Boolean,  // Did reviewer purchase a car?
  purchase_date: String,   // Purchase date
  car_make:      String,   // Car brand reviewed (e.g., "Toyota")
  car_model:     String,   // Car model reviewed
  car_year:      Number    // Car year reviewed
}
```

---

## 🐳 Deployment

### Docker Compose

The `server/database/docker-compose.yml` spins up the Node.js API and MongoDB together:

```bash
cd server/database
docker-compose up --build -d
```

| Service | Image | Port |
|---------|-------|------|
| `mongo_db` | `mongo:latest` | 27017 |
| `api` | Node.js app build | 3030 |

### Docker — Django Application

```bash
cd server
docker build -t bestcars-django .
docker run -p 8000:8000 \
  -e backend_url=http://api:3030 \
  -e sentiment_analyzer_url=http://sentiment:5050/ \
  bestcars-django
```

### Kubernetes

A Kubernetes deployment manifest is provided at `server/deployment.yaml`:

```yaml
Deployment: dealership
Replicas: 1
Image: us.icr.io/sn-labs-<username>/dealership:latest
Port: 8000
Update Strategy: RollingUpdate (maxSurge: 25%, maxUnavailable: 25%)
```

Apply to a cluster:

```bash
kubectl apply -f server/deployment.yaml
kubectl get pods
kubectl get services
```

---

## ⚙️ CI/CD Pipeline

Defined in `.github/workflows/main.yml`, the pipeline runs on every push and pull request to `main`/`master`.

### Jobs

| Job | Runtime | Steps |
|-----|---------|-------|
| `lint_python` | ubuntu-latest | Set up Python 3.12 → Install flake8 → Lint all `.py` files |
| `lint_js` | ubuntu-latest | Set up Node.js 14 → Install JSHint → Lint all `.js` files in `server/database/` |

### Triggers

```yaml
on:
  push:
    branches: [master, main]
  pull_request:
    branches: [master, main]
```

---

## 🎯 Skills & Technologies Showcased

### Full-Stack Web Development
- **React (v18)** — Component-based UI, hooks (`useState`, `useEffect`), conditional rendering, form handling
- **React Router v6** — Client-side SPA routing with dynamic parameters (`:id`, `:state`)
- **Django** — MVC/MVT pattern, ORM, class-based views, URL routing, session authentication
- **REST API Design** — Stateless endpoints, proper HTTP methods (GET/POST), JSON request/response

### Microservices Architecture
- **Service decomposition** — Three independently deployable services (Django, Node.js, Flask)
- **Inter-service communication** — HTTP-based API proxying between Django and downstream services
- **Separation of concerns** — Each service owns its own data store and logic

### Backend Engineering
- **Node.js + Express** — Building a lightweight, fast REST API
- **Flask** — Rapid prototyping of a Python microservice
- **Django REST** — Handling authentication, business logic, and data persistence
- **Gunicorn** — Production WSGI server configuration

### Databases
- **MongoDB** — Schema design, Mongoose ODM, document-oriented modeling
- **SQLite3 / Django ORM** — Relational modeling, migrations, foreign keys, model validation
- **Polyglot persistence** — Using the right database for the right job (NoSQL for reviews, SQL for structured car data)

### NLP / AI Integration
- **NLTK VADER** — Rule-based sentiment analysis on free-text reviews
- **AI-powered features** — Real-time sentiment badges (Positive 😊 / Neutral 😐 / Negative 😞) on the review UI

### DevOps & Cloud
- **Docker** — Writing `Dockerfile`, multi-service `docker-compose.yml`, image layering best practices
- **Kubernetes** — Writing deployment manifests, rolling update strategies, container port mapping
- **GitHub Actions** — Defining CI/CD workflows (YAML), matrix builds, automated quality gates

### Software Engineering Practices
- **Environment configuration** — `.env` files, python-dotenv, separation of config from code
- **Code quality** — Automated linting (flake8 for Python, JSHint for JavaScript)
- **CORS handling** — Properly configured cross-origin policies for microservice communication
- **CSRF protection** — Django CSRF tokens managed for API calls

---

## 📄 License

This project is licensed under the terms of the [LICENSE](LICENSE) file included in this repository.

---

## 👤 Author

**Rafael A. G. N.**  
IBM Full-Stack Developer Professional Certificate — Capstone Project

[![GitHub](https://img.shields.io/badge/GitHub-rafael--a--g--n-181717?logo=github)](https://github.com/rafael-a-g-n)
