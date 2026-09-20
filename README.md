# CollabSphere
# CollabSphere

A full-stack collaborative project management platform for teams to manage workspaces, projects, tasks, communication, and notifications in real time.

## Features

* User registration, login, logout, and JWT-based authentication
* Workspace creation and member management
* Workspace invitations with role-based access
* Project management within workspaces
* Task creation, assignment, deadlines, priorities, and status tracking
* Real-time team chat using Socket.IO
* Kafka-based asynchronous chat processing
* Redis and BullMQ-based notification processing
* Real-time notification delivery
* Dashboard with workspace, project, and task statistics
* AI assistance powered by Google Gemini
* Dockerized backend with MongoDB, Redis, and Kafka
* GitHub Actions CI/CD for deployment

## Tech Stack

### Frontend

* React
* Vite
* Tailwind CSS
* React Router
* Axios
* Socket.IO Client
* Framer Motion

### Backend

* Node.js
* Express.js
* MongoDB
* Mongoose
* JWT
* Socket.IO
* Apache Kafka
* Redis
* BullMQ

### AI

* Google Gemini API

### DevOps

* Docker
* Docker Compose
* GitHub Actions
* AWS EC2
* Nginx

## Project Structure

```text
CollabSphere/
├── Backend/
│   ├── controller/
│   ├── db/
│   ├── middlewares/
│   ├── models/
│   ├── routes/
│   ├── utils/
│   ├── app.js
│   ├── server.js
│   ├── index.js
│   ├── Dockerfile
│   └── docker-compose.yml
│
├── Frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── context/
│   │   ├── pages/
│   │   └── services/
│   └── package.json
│
└── .github/
    └── workflows/
```

## Architecture

CollabSphere uses a modular full-stack architecture.

* **React** handles the user interface and client-side state.
* **Express.js** provides REST APIs for authentication, workspaces, projects, tasks, notifications, chat, and dashboard data.
* **MongoDB** stores application data.
* **Socket.IO** provides real-time communication for chat and notifications.
* **Kafka** processes chat events asynchronously before persisting and broadcasting them.
* **Redis + BullMQ** handle asynchronous notification jobs.
* **Gemini API** provides AI assistance from the task-management interface.
* **Docker Compose** runs the backend services and infrastructure.
* **GitHub Actions** automates frontend and backend deployment.

## Getting Started

### Prerequisites

Make sure you have:

* Node.js 22+
* Docker and Docker Compose
* MongoDB, Redis, and Kafka if running services manually
* A Google Gemini API key for AI functionality

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/CollabSphere.git
cd CollabSphere
```

### 2. Configure the backend

Create:

```text
Backend/.env
```

Add the required environment variables:

```env
PORT=3000
DB_NAME=collabsphere

ACCESS_TOKEN_SECRET=your_access_token_secret
ACCESS_TOKEN_EXPIRY=your_access_token_expiry

REFRESH_TOKEN_SECRET=your_refresh_token_secret
REFRESH_TOKEN_EXPIRY=your_refresh_token_expiry

FRONTEND_URL=http://localhost:5173

REDIS_HOST=redis
REDIS_PORT=6379
```

### 3. Configure the frontend

Create:

```text
Frontend/.env
```

```env
VITE_SERVER=localhost:3000
VITE_GEMINI_API_KEY=your_gemini_api_key
```

### 4. Start backend services

```bash
cd Backend
docker compose up -d
```

The backend will run on:

```text
http://localhost:3000
```

### 5. Start the frontend

Open another terminal:

```bash
cd Frontend
npm install
npm run dev
```

The frontend will be available at:

```text
http://localhost:5173
```

## Application Flow

```text
User
 │
 ├── Register / Login
 │
 ▼
Workspace
 │
 ├── Members
 ├── Invitations
 ├── Projects
 │    └── Tasks
 │         ├── Assignment
 │         ├── Priority
 │         ├── Deadline
 │         └── Status
 │
 ├── Real-time Chat
 │
 ├── Notifications
 │
 └── Dashboard
```

## Real-Time Architecture

### Chat

Chat messages are handled asynchronously:

```text
User
  │
  ▼
REST API
  │
  ▼
Kafka Producer
  │
  ▼
Kafka Topic
  │
  ▼
Kafka Consumer
  │
  ├── Store message in MongoDB
  │
  └── Broadcast through Socket.IO
```

### Notifications

Notifications use a background queue:

```text
Application Event
      │
      ▼
   BullMQ
      │
      ▼
    Redis
      │
      ▼
Notification Worker
      │
      ├── Store notification in MongoDB
      │
      └── Send through Socket.IO
```

This keeps notification processing separate from the main request flow.

## Authentication

The backend uses JWT-based authentication.

* Passwords are hashed using bcrypt.
* Access tokens authenticate protected API requests.
* Refresh tokens are supported for renewing authentication.
* HTTP cookies are used for token handling.
* Socket.IO connections also validate authentication before establishing a connection.

## Deployment

The project includes GitHub Actions workflows for automated deployment.

### Backend

```text
GitHub
   ↓
GitHub Actions
   ↓
Docker Image
   ↓
Docker Hub
   ↓
AWS EC2
```

### Frontend

```text
GitHub
   ↓
GitHub Actions
   ↓
Build React Application
   ↓
Deploy to Nginx
   ↓
AWS EC2
```

Deployment credentials and API keys are provided through GitHub Secrets rather than being committed to the repository.

## Environment Variables

Never commit `.env` files or API keys to the repository.

Required configuration includes:

* JWT secrets
* MongoDB configuration
* Redis configuration
* Frontend URL
* Backend URL
* Gemini API key
* Deployment credentials

## License

This project is currently released without a specified open-source license.
