
# 💬 ChatX

### Real-time room-based chat built with React, Spring Boot, WebSockets, and MongoDB

A full-stack chat application where users can create a chat room or join
an existing room and exchange messages in real time.

[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=black)](https://react.dev/)
[![Vite](https://img.shields.io/badge/Vite-6-646CFF?logo=vite&logoColor=white)](https://vite.dev/)
[![Spring
Boot](https://img.shields.io/badge/Spring%20Boot-3.5-6DB33F?logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![MongoDB](https://img.shields.io/badge/MongoDB-Database-47A248?logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![WebSocket](https://img.shields.io/badge/Real--time-STOMP%20%2B%20SockJS-333333)](https://stomp.github.io/)
:::

------------------------------------------------------------------------

## ✨ Features

-   **Create chat rooms** --- create a room using a room ID.
-   **Join existing rooms** --- enter a room ID and display name to
    join.
-   **Real-time messaging** --- send and receive messages over STOMP
    using SockJS and Spring WebSocket.
-   **Message persistence** --- room messages are stored in MongoDB.
-   **Message history** --- load previously stored messages when opening
    a room.
-   **Paginated history endpoint** --- request room messages using
    `page` and `size` parameters.
-   **Room-aware conversations** --- messages are broadcast to
    subscribers of the corresponding room.
-   **Responsive single-page frontend** --- React, React Router,
    Tailwind CSS, and toast notifications.

## 🧰 Tech Stack

| Area | Technologies |
|---|---|
| **Frontend** | React 18, Vite 6, React Router |
| **Styling & UI** | Tailwind CSS, React Icons, React Hot Toast |
| **HTTP Requests** | Axios |
| **Real-Time Communication** | STOMP.js, SockJS |
| **Backend** | Java, Spring Boot |
| **Backend APIs** | Spring Web, Spring WebSocket |
| **Database** | MongoDB, Spring Data MongoDB |
| **Build Tools** | npm, Maven |

## 🗂️ Project structure

``` text
chatx/
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/       # Join/create room and chat UI
│   │   ├── config/           # Routes, API client, helpers
│   │   ├── context/          # Shared chat state
│   │   └── services/         # Room API calls
│   ├── package.json
│   └── vite.config.js
├── backend/
│   ├── src/main/java/com/substring/chat/
│   │   ├── config/           # WebSocket and app configuration
│   │   ├── controllers/      # Room REST API and chat messaging
│   │   ├── entities/         # Room and Message models
│   │   ├── repositories/     # MongoDB repository
│   │   └── playload/         # Message request payload
│   ├── src/main/resources/
│   │   └── application.properties
│   ├── pom.xml
│   └── mvnw / mvnw.cmd
└── README.md
```

## ✅ Prerequisites

Install the following before running the application:

-   **Java 25** (the backend Maven configuration targets Java 25)
-   **Node.js and npm** (use a current supported Node.js LTS release)
-   **MongoDB** running locally or a MongoDB connection URI
-   **Git** (to clone the repository)

> The default backend configuration uses
> `mongodb://localhost:27017/chatapp`. Ensure MongoDB is running before
> starting the backend.

## 🚀 Run locally

Open two terminals from the repository root.

### 1. Start the backend

Make sure MongoDB is running, then run:

**Windows (Command Prompt):**

``` bat
cd backend
mvnw.cmd spring-boot:run
```

**macOS / Linux:**

``` bash
cd backend
chmod +x mvnw
./mvnw spring-boot:run
```

The Spring Boot API is configured for `http://localhost:8080`.

### 2. Start the frontend

In a second terminal:

``` bash
cd frontend
npm install
npm run dev
```

Open the Vite URL printed in the terminal, usually
**http://localhost:5173**.

### 3. Try ChatX

1.  Open the frontend in your browser.
2.  Enter a display name and a room ID.
3.  Select **Create Room** to create a new room, or **Join Room** to
    enter an existing one.
4.  Open the same room from another browser tab or window with a
    different display name to test real-time messaging.

## 🔌 API reference

The backend exposes the following room endpoints on
`http://localhost:8080`.

  -----------------------------------------------------------------------------------
  Method                  Endpoint                            Purpose
  ----------------------- ----------------------------------- -----------------------
  `POST`                  `/api/v1/rooms`                     Create a room. Send the
                                                              room ID as plain text
                                                              in the request body.

  `GET`                   `/api/v1/rooms/{roomId}`            Find a room and join
                                                              it.

  `GET`                   `/api/v1/rooms/{roomId}/messages`   Retrieve room message
                                                              history.
  -----------------------------------------------------------------------------------

### Message history parameters

`GET /api/v1/rooms/{roomId}/messages` supports:

-   `page` --- zero-based page index; defaults to `0`.
-   `size` --- number of messages requested; defaults to `20` in the
    backend.

Example:

``` http
GET http://localhost:8080/api/v1/rooms/general/messages?page=0&size=20
```

The frontend currently requests a history size of 50 by default.

## ⚡ Real-time messaging

The backend configures a SockJS endpoint at `/chat`, with STOMP
destinations:

  Purpose                Destination
  ---------------------- -----------------------------
  Establish connection   `/chat`
  Send a message         `/app/sendMessage/{roomId}`
  Subscribe to a room    `/topic/room/{roomId}`

The backend currently allows the frontend origin
`http://localhost:5173`. If you run the frontend on a different host or
port, update the CORS/WebSocket origin configuration in
`backend/src/main/java/com/substring/chat/config/AppConstants.java`.

The frontend's API base URL is currently `http://localhost:8080`,
configured in `frontend/src/config/AxiosHelper.js`. Update it when
deploying to another environment.

## ⚙️ Configuration

### MongoDB

The current setting is in
`backend/src/main/resources/application.properties`:

``` properties
spring.data.mongodb.uri=mongodb://localhost:27017/chatapp
```

Change the URI if you use a remote database. Keep credentials out of
source control; use environment-specific configuration or environment
variables for deployment.

### Frontend and backend URLs

For local development, the frontend expects the backend at
`http://localhost:8080`, and the backend allows the frontend at
`http://localhost:5173`. Update both settings consistently when hosting
the application elsewhere.

## 🧪 Useful commands

### Frontend

``` bash
cd frontend
npm run dev       # Start development server
npm run build     # Create production build
npm run preview   # Preview production build
npm run lint      # Run ESLint
```

### Backend

``` bash
cd backend
```

**Windows:**

``` bat
mvnw.cmd test
mvnw.cmd package
```

**macOS / Linux:**

``` bash
./mvnw test
./mvnw package
```

## 🔒 Security notes

-   Do not commit `.env` files, database credentials, access tokens, or
    other secrets.
-   Configure production CORS origins explicitly rather than allowing
    arbitrary origins.
-   Use authenticated and authorized room access before exposing the
    application publicly.
-   Use HTTPS/WSS and a secured MongoDB deployment in production.

## 🛣️ Potential improvements

-   User authentication and room access controls
-   Typing indicators and online presence
-   Message delivery/read status
-   File and image attachments
-   Environment-based configuration for deployment
-   Automated tests for room and messaging flows
-   Docker-based local development and deployment

## 🤝 Contributing

Contributions and suggestions are welcome.

1.  Fork the repository.
2.  Create a feature branch: `git checkout -b feat/your-feature`.
3.  Commit your changes: `git commit -m "Add your feature"`.
4.  Push the branch and open a pull request.

## 📄 License

No license is currently specified. Add a `LICENSE` file if you intend to
publish the project for reuse, and choose a license that matches your
goals.

------------------------------------------------------------------------


**Built with React, Spring Boot, WebSockets, and MongoDB.**
:::
