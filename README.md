# 🗳️ QuickPoll — A Real-Time Polling Application Built with Go and Server-Sent Events

A lightweight **real-time polling web application** written in Go.  
Live results are delivered using **Server-Sent Events (SSE)** — no WebSockets, no external dependencies.

![Go](https://img.shields.io/badge/Go-1.21-00ADD8?style=for-the-badge&logo=go&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-CDN-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)
![Render](https://img.shields.io/badge/Render-Deployed-46E3B7?style=for-the-badge&logo=render&logoColor=white)

This project is fully self-contained, uses in-memory storage, and is ideal as:
- a learning example,
- an MVP template,
- a demonstration of Go concurrency and SSE.

---

## 🚀 Features

- Create polls with multiple options
- Vote without page reloads
- Live result updates via SSE
- Vote percentage and total vote calculation
- Poll expiration support
- Thread-safe in-memory storage
- Simple REST API
- Comprehensive unit tests

---

## 🧱 Tech Stack

- **Go 1.21**
- `net/http`
- `html/template`
- **Server-Sent Events (SSE)**
- Tailwind CSS (via CDN)
- In-memory storage

No database.  
No JavaScript frameworks.  
No third-party Go libraries.

---

## 📂 Project Structure

```
go-real-time-poll/
├── main.go            # Server, business logic, SSE, and HTML templates
├── main_test.go       # Unit tests (store, poll, broadcaster)
├── go.mod
├── render.yaml        # Render deployment config
├── .gitignore
└── README.md
```

---

## ▶️ Run Locally

```bash
go run .
```

Open in your browser:

```
http://localhost:8080
```

---

## 🧪 Run Tests

```bash
go test -v
```

Tests cover:
- ID generation
- Poll storage
- Concurrent voting
- Percentage calculations
- SSE broadcaster logic

---

## 🌐 HTTP Endpoints

### Web
- `/` — list all polls
- `/create` — create a new poll
- `/poll/{id}` — poll page
- `/vote/{id}` — submit a vote (POST)
- `/events/{id}` — SSE stream

### API
- `/api/polls` — list all polls (JSON)

---

## 🔧 API Usage (cURL Examples)

### List all polls

```bash
curl http://localhost:8080/api/polls
```

---

### Vote for an option

```bash
curl -X POST http://localhost:8080/vote/{POLL_ID}   -H "Accept: application/json"   -d "option={OPTION_ID}"
```

Example:

```bash
curl -X POST http://localhost:8080/vote/572d642b   -H "Accept: application/json"   -d "option=05a2acd0"
```

---

### Subscribe to real-time updates (SSE)

```bash
curl http://localhost:8080/events/{POLL_ID}
```

This will keep the connection open and stream poll updates as votes are submitted.

> ⚠️ **Note about cURL and SSE**
>
> When using `curl` with Server-Sent Events, output is **buffered by default**.
> To receive events **in real time**, you must disable buffering:
>
> ```bash
> curl -N http://localhost:8080/events/{POLL_ID}
> ```
>
> Without the `-N` (`--no-buffer`) flag, events will only appear after the connection is closed.

---

## 📡 Real-Time Architecture

For each poll:
- clients subscribe via `/events/{pollID}`
- every vote triggers a broadcast
- all connected clients receive updates instantly

Implemented using native browser `EventSource`.

---

## 🧠 Ideas for Improvement

- Persistent storage (PostgreSQL / SQLite)
- Authentication
- WebSocket implementation
- Vote deduplication
- Admin dashboard
- Docker support

---

## ☁️ Deployment (Render)

The project is ready to deploy on **Render** out of the box.

`render.yaml`:

```yaml
services:
  - type: web
    name: go-real-time-poll
    env: go
    plan: free
    buildCommand: |
      if [ ! -f go.mod ]; then
        go mod init app
      fi
      go mod tidy
      go build -o app .
    startCommand: ./app
```

---

## Deploy in 10 seconds

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy)
