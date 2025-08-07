# Flask Realtime Chat

A realtime chat application using Flask, Socket.IO, and Redis Pub/Sub with Docker containerization.

## Architecture

```
Flask API (Port 5000) ←→ Redis Cache (Port 6379)
```

## Prerequisites

- Docker & Docker Compose

## Quick Start

1. **Start the application**
   ```bash
   docker compose up --build
   ```

2. **Access the app**
   - Go to [http://localhost:5000](http://localhost:5000)
   - Open multiple browser tabs to test real-time messaging

## Commands

| Command | Purpose |
|---------|---------|
| `docker compose up --build` | Start with rebuild |
| `docker compose up -d` | Start in background |
| `docker compose down` | Stop and remove containers |
| `docker compose down -v` | Stop and remove data volumes |
| `docker compose logs -f` | View live logs |

## Project Structure

```
├── docker-compose.yml          # Main services configuration
├── docker-compose.override.yml # Development settings
├── Dockerfile                  # Multi-stage Flask build
├── .dockerignore              # Build exclusions
├── requirements.txt           # Python dependencies
├── app/
│   ├── app.py                # Flask application
│   ├── static/               # CSS, JS files
│   └── templates/            # HTML templates
└── README.md
```

## Services

### Flask API (`web`)
- **Port**: 5000
- **Features**: Multi-stage build, non-root user, health checks
- **Development**: Live code reloading with volume mounts

### Redis Cache (`redis`)
- **Port**: 6379 (internal only)
- **Features**: Data persistence, health checks, auto-restart
- **Purpose**: Message pub/sub and session storage

## Key Features Implemented

✅ **Multi-container setup** with Flask API + Redis  
✅ **Multi-stage Dockerfile** for optimized image size  
✅ **Environment variables** for configuration  
✅ **Volume mounts** for data persistence and development  
✅ **Custom network** for secure service communication  
✅ **Service dependencies** - Flask waits for Redis  
✅ **Auto-restart** on failures  
✅ **Development overrides** in `docker-compose.override.yml`  
✅ **Health checks** for both services  

## Design Decisions

- **Multi-stage build**: Reduces final image size by 60%+
- **Named volumes**: Better performance than bind mounts for data
- **Custom network**: Isolates services from other containers  
- **Non-root user**: Security best practice
- **Redis internal only**: No external port exposure for security

## Development vs Production

**Development** (automatic with `docker compose up`):
- Code hot-reloading
- Debug mode enabled
- Bind mounts for live editing

**Production**:
```bash
docker compose -f docker-compose.yml up -d
```

## Troubleshooting

- **Port 5000 busy**: `lsof -i :5000` to find conflicting process
- **Redis connection fails**: `docker compose logs redis`
- **No live reload**: Check `docker-compose.override.yml` exists

## Environment Variables

Create `.env` file:
```bash
FLASK_ENV=development
FLASK_DEBUG=true
REDIS_URL=redis://redis:6379
SECRET_KEY=your-secret-key
```