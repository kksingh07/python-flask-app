# Flask App

A minimal Flask web application built for learning containerization


## Features

- Responsive landing page with modern glassmorphism UI
- Two Dockerfiles — simple and multistage (distroless)

## Tech Stack

| Component | Technology |
| :--- | :--- |
| Framework | Flask 3.1.1 |
| Runtime | Python 3.14 |
| Container | Docker (python-slim / distroless) |
| Deploy | AWS ECS |

## Project Structure

```text
flask-app-ecs/
├── app.py                  # Flask app with routes
├── run.py                  # Entry point (host 0.0.0.0, port 80)
├── requirements.txt        # Python dependencies
├── templates/
│   └── index.html          # Landing page
├── Dockerfile              # Simple single-stage build
└── Dockerfile-multi        # Multistage build with distroless
```

## Quick Start

### Run locally

```bash
pip install -r requirements.txt
python run.py
```

App runs at http://localhost:80.

### Run with Docker

**Simple build:**

```bash
docker build -t flask-app .
docker run -p 80:80 flask-app
```

**Multistage build (smaller, production-grade):**

```bash
docker build -f Dockerfile-multi -t flask-app .
docker run -p 80:80 flask-app
```

## Dockerfiles Explained

### Simple (`Dockerfile`)

Single-stage build using `python:3.14-slim`. Straightforward — copies everything, installs dependencies, runs the app. Good for development and learning.

### Multistage (`Dockerfile-multi`)

Two-stage build:

1. **Builder stage** — installs dependencies into a separate directory using `python:3.14-slim`
2. **Final stage** — copies only the app and deps into a `distroless` image

Benefits:

- Smaller final image (no pip, no shell, no OS utilities)
- Reduced attack surface — distroless images contain only the app and its runtime
- Better layer caching — dependencies are copied before source code





