# Microservices Orchasterisation Example Using Docker Swarm
Deploying a **microservices architecture** using **Docker Swarm**, which is Docker's native clustering and orchestration tool.

### Overview
In this example, we will create a simple microservices architecture with two services:
1. **API Gateway** - Acts as the entry point for the users.
2. **Backend Service** - A simple service that provides data to the API Gateway.

We'll deploy these services using Docker Swarm, where we define services, networks, and stacks in Docker.

### Prerequisites
1. **Docker** installed.
2. **Docker Swarm** initialized.

### Step-by-Step Guide

#### 1. **Initialize Docker Swarm**

To start with Docker Swarm, we need to initialize it on your machine. This can be done with the following command:

```bash
docker swarm init
```

This initializes your local machine as a manager node in the swarm. If you want to add more nodes to the swarm, Docker will provide a command with a token to join other machines.

#### 2. **Create Microservices Code**

We'll create two microservices in Python using **Flask** and then Dockerize them.

##### 2.1 **Backend Service**

Create the backend service code:

**backend.py**:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from the Backend Service!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
```

Next, create the **Dockerfile** for the backend service:

**Dockerfile (for Backend)**:

```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY backend.py /app

RUN pip install flask

CMD ["python", "backend.py"]
```

##### 2.2 **API Gateway**

Now, create the API Gateway that will communicate with the backend service:

**api_gateway.py**:

```python
from flask import Flask
import requests

app = Flask(__name__)

@app.route('/')
def hello():
    backend_response = requests.get('http://backend-service:5000')
    return f"API Gateway: {backend_response.text}"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=8080)
```

Now, create the **Dockerfile** for the API Gateway:

**Dockerfile (for API Gateway)**:

```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY api_gateway.py /app

RUN pip install flask requests

CMD ["python", "api_gateway.py"]
```

#### 3. **Build the Docker Images**

Now, build the Docker images for both services.

```bash
# Build the backend service Docker image
docker build -t backend-service ./path_to_backend_service

# Build the API Gateway Docker image
docker build -t api-gateway ./path_to_api_gateway
```

#### 4. **Create Docker Compose for Swarm**

For Docker Swarm, we will use a **Docker Compose** file to define the services, networks, and volumes. This file will also contain the configuration for Docker Swarm.

**docker-compose.yml**:

```yaml
version: '3.7'

services:
  backend-service:
    image: backend-service  # use your local image or a registry image
    deploy:
      replicas: 2
      restart_policy:
        condition: on-failure
    networks:
      - app-network
    ports:
      - "5000:5000"

  api-gateway:
    image: api-gateway  # use your local image or a registry image
    deploy:
      replicas: 2
      restart_policy:
        condition: on-failure
    networks:
      - app-network
    ports:
      - "8080:8080"
    depends_on:
      - backend-service

networks:
  app-network:
    driver: overlay
```

In this example:
- The **backend-service** and **api-gateway** will each be deployed with **2 replicas** for load balancing.
- Both services are connected to a custom **overlay network** (`app-network`), which allows communication between services in the swarm.
- The API Gateway depends on the backend service to be up and running.

#### 5. **Deploy the Stack to Docker Swarm**

Now, deploy the application to Docker Swarm using the `docker stack` command:

```bash
# Deploy the stack to Docker Swarm
docker stack deploy -c docker-compose.yml my_microservices
```

Docker will create the services, deploy the specified number of replicas, and ensure that the services are up and running.

To check the status of the deployed services, use:

```bash
docker stack services my_microservices
```

#### 6. **Access the Microservices**

After the services are deployed, you can access the services through the exposed ports.

- **API Gateway**: Open a browser and visit `http://localhost:8080`.
  - This should show the message from the **API Gateway**, which in turn fetches the response from the **Backend Service**.

You can also check the logs of a specific service to make sure everything is working:

```bash
docker service logs my_microservices_api-gateway
```

#### 7. **Scaling Services**

One of the key features of Docker Swarm is scaling services. You can easily scale up or down the number of replicas for a service.

To scale the **backend-service** to 5 replicas:

```bash
docker service scale my_microservices_backend-service=5
```

This will add more instances of the backend service to handle increased load.

#### 8. **Updating Services**

You can also update the services in a swarm. For instance, if you update the Docker image for the **backend-service**, you can deploy the new version with the following command:

```bash
docker service update --image backend-service:latest my_microservices_backend-service
```

This will update the backend service with the new image while keeping the system running.

#### 9. **Remove the Stack**

Once you're done with testing, you can remove the stack by running:

```bash
docker stack rm my_microservices
```

This will stop and remove the services defined in the stack.

---

### Summary

In this example:
- We created two microservices (API Gateway and Backend Service).
- We dockerized both services using Dockerfiles.
- We defined a Docker Compose file that can be used for Docker Swarm to deploy and manage the services.
- We deployed the stack to Docker Swarm using `docker stack deploy`.
- We accessed the microservices via HTTP and demonstrated scaling.

Docker Swarm makes it easy to orchestrate and manage multi-container applications with services that can scale automatically based on demand.