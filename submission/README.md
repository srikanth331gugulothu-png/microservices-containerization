# Microservices Containerization Assessment

A Node.js microservices application containerized using Docker and Docker Compose.

## Architecture

| Service | Container Port | Local URL |
|---|---:|---|
| User Service | 3000 | http://localhost:3000 |
| Product Service | 3001 | http://localhost:3001 |
| Order Service | 3002 | http://localhost:3002 |
| Gateway Service | 3003 | http://localhost:3003 |

All services share the `microservices-network` Docker network.
The Gateway communicates with the other services using their
Docker Compose service names.

## Project Structure

```text
Microservices-Task/
├── Microservices/
│   ├── user-service/
│   │   ├── Dockerfile
│   │   ├── app.js
│   │   └── package.json
│   ├── product-service/
│   │   ├── Dockerfile
│   │   ├── app.js
│   │   └── package.json
│   ├── order-service/
│   │   ├── Dockerfile
│   │   ├── app.js
│   │   └── package.json
│   └── gateway-service/
│       ├── Dockerfile
│       ├── app.js
│       └── package.json
├── docker-compose.yml
├── README.md
└── screenshots/
```

## Prerequisites

- Docker Desktop
- Docker Compose
- Git

## Setup and Start

Clone the repository, navigate to the project directory, and run:

```bash
docker compose up --build -d
docker compose ps
```

## API Endpoints

### Health checks

- `GET http://localhost:3000/health`
- `GET http://localhost:3001/health`
- `GET http://localhost:3002/health`
- `GET http://localhost:3003/health`

### Users

```bash
curl http://localhost:3000/users
curl http://localhost:3003/api/users
```

### Products

```bash
curl http://localhost:3001/products
curl http://localhost:3003/api/products
```

### Orders

```bash
curl http://localhost:3002/orders
curl http://localhost:3003/api/orders
```

Create an order through the Gateway:

```bash
curl -X POST http://localhost:3003/api/orders \
  -H "Content-Type: application/json" \
  -d '{"userId":1,"productId":1}'
```

Retrieve orders:

```bash
curl http://localhost:3003/api/orders
```

Orders are stored in memory and are lost when the Order Service restarts.

## Stop the Application

```bash
docker compose down
```

## Troubleshooting

### Docker daemon unavailable
Start Docker Desktop and wait for the engine to finish starting.

### Port already in use
Check whether another process is using ports 3000–3003.

### Container exits or restarts
Inspect service logs:

```bash
docker compose logs
docker compose logs gateway-service
docker compose logs order-service
```

### Gateway cannot reach a service
Confirm all containers are running and that the Gateway uses the Compose service names:
`user-service`, `product-service`, and `order-service`.

## Screenshots

Place screenshots of running containers and successful API tests
in the `screenshots/` directory.

## Author

Add your name and submission repository URL here.
