# Microservices Containerization Submission

This project contains the Dockerized implementation of a microservices-based application containing four Node.js services orchestrated using Docker Compose:

1. **User Service** (Port 3000)
2. **Product Service** (Port 3001)
3. **Order Service** (Port 3002)
4. **Gateway Service** (Port 3003)

---

## Directory Structure
```
submission/
├── user-service/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
├── product-service/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
├── order-service/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
├── gateway-service/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
├── docker-compose.yml
└── README.md
```

---

## Setup & Running Instructions

### Prerequisites
- Docker and Docker Compose installed.
- Node.js (optional, for local testing outside containers).

### Running with Docker Compose
1. Navigate to the `submission/` directory:
   ```bash
   cd submission
   ```
2. Build and start the services in detached mode:
   ```bash
   docker-compose up --build -d
   ```
3. Verify that the containers are running:
   ```bash
   docker-compose ps
   ```

---

## How to Test Each Service

You can test individual services directly or test them via the Gateway Service.

### 1. Direct Service Endpoints

* **User Service**
  - URL: `http://localhost:3000/users`
  - Command:
    ```bash
    curl http://localhost:3000/users
    ```

* **Product Service**
  - URL: `http://localhost:3001/products`
  - Command:
    ```bash
    curl http://localhost:3001/products
    ```

* **Order Service**
  - URL: `http://localhost:3002/orders`
  - Command:
    ```bash
    curl http://localhost:3002/orders
    ```

---

### 2. Gateway Service Endpoints (Aggregated)

The Gateway Service proxies requests to the other microservices.

* **Health Check**
  - Command: `curl http://localhost:3003/health`
* **Fetch Users via Gateway**
  - Command: `curl http://localhost:3003/api/users`
* **Fetch Products via Gateway**
  - Command: `curl http://localhost:3003/api/products`
* **Fetch Orders via Gateway**
  - Command: `curl http://localhost:3003/api/orders`
* **Create Order via Gateway**
  - Command (Windows Powershell):
    ```powershell
    Invoke-RestMethod -Uri http://localhost:3003/api/orders -Method Post -Body '{"userId": 1, "productId": 2}' -ContentType "application/json"
    ```
  - Command (Bash / Linux / macOS):
    ```bash
    curl -X POST -H "Content-Type: application/json" -d '{"userId": 1, "productId": 2}' http://localhost:3003/api/orders
    ```

---

## Troubleshooting Tips

1. **Port Conflicts**:
   - If any of the ports (3000, 3001, 3002, 3003) are already in use on your host machine, stop the occupying processes or edit the host ports in `docker-compose.yml`.
2. **Rebuilding Containers**:
   - If you make changes to the source code, rebuild the images by running:
     ```bash
     docker-compose up --build -d
     ```
3. **Logs**:
   - To view logs of all services:
     ```bash
     docker-compose logs
     ```
   - To follow logs of a specific service:
     ```bash
     docker-compose logs -f <service-name>
     ```
4. **Stopping Services**:
   - To stop the services and remove containers/networks:
     ```bash
     docker-compose down
     ```
