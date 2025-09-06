# Docker Setup

This project includes Docker Compose configuration for both production and development environments.

## Services

- **tldraw-client**: The main React application served via nginx (production)
- **tldraw-dev**: Development server with hot reloading (development only)
- **y-websocket**: WebSocket server for real-time collaboration features

## Quick Start

### Production Environment

```bash
# Build and start production services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

The application will be available at:
- Main app: http://localhost:3046
- WebSocket server: ws://localhost:1234

### Development Environment

```bash
# Start development environment with hot reloading
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up

# Or in detached mode
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up -d

# View logs
docker-compose -f docker-compose.yml -f docker-compose.dev.yml logs -f tldraw-dev

# Stop development services
docker-compose -f docker-compose.yml -f docker-compose.dev.yml down
```

The development application will be available at:
- Main app: http://localhost:5173 (with hot reloading)
- WebSocket server: ws://localhost:1234

## Environment Variables

You can customize the setup by creating a `.env` file:

```bash
# Production app port
TLDRAW_CLIENT_PORT=3046

# Development app port  
TLDRAW_DEV_PORT=5173

# WebSocket server port
WEBSOCKET_PORT=1234

# Node environment
NODE_ENV=production
```

Then reference them in your docker-compose.yml:

```yaml
ports:
  - "${TLDRAW_CLIENT_PORT:-3046}:3046"
```

## Volumes

- `websocket_data`: Persists WebSocket server data for collaboration state

## Network

All services communicate via the `tldraw-network` bridge network.

## Troubleshooting

### Common Issues

1. **Port conflicts**: Change ports in docker-compose.yml if 3046, 5173, or 1234 are already in use
2. **Build failures**: Ensure all files (especially nginx.conf.template) are present
3. **WebSocket connection issues**: Check that the y-websocket service is running and accessible

### Useful Commands

```bash
# Rebuild services
docker-compose build --no-cache

# View service status
docker-compose ps

# Access container shell
docker-compose exec tldraw-client sh
docker-compose exec y-websocket sh

# View specific service logs
docker-compose logs -f tldraw-client
docker-compose logs -f y-websocket

# Clean up everything (including volumes)
docker-compose down -v --rmi all
```

## File Structure

```
├── docker-compose.yml          # Main compose file
├── docker-compose.dev.yml      # Development overrides
├── Dockerfile                  # Production build
├── Dockerfile.dev              # Development build
├── nginx.conf.template         # Nginx configuration
└── DOCKER.md                   # This documentation
```
