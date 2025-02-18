# Microservices Orchasterisation Example 1

Example of creating a basic microservices architecture in Minikube using Kubernetes and `kubectl`.

### Overview
We'll create two microservices:
1. **API Gateway** (which acts as the entry point)
2. **Backend Service** (the microservice that handles the logic and returns a response)

We'll use Minikube to set up a local Kubernetes cluster and deploy our microservices. We'll also use `kubectl` to interact with Kubernetes.

### Prerequisites
- **Minikube** installed (for running a local Kubernetes cluster)
- **kubectl** installed (to interact with the cluster)

### Step-by-Step Guide

#### 1. **Start Minikube**

Start your Minikube cluster with the following command:

```bash
minikube start
```

#### 2. **Create Microservices**

Let's create two simple microservices.

##### 2.1 **Backend Service**

First, create a simple backend service. This service will run a basic Python Flask app that returns a simple message.

**backend.py:**

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from the Backend Service!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
```

To run this Flask app inside a container, we need to create a Dockerfile.

**Dockerfile:**

```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY . /app

RUN pip install flask

CMD ["python", "backend.py"]
```

Build the Docker image for the backend:

```bash
docker build -t backend-service .
```

##### 2.2 **API Gateway**

Now create a simple API Gateway that communicates with the backend service. Here’s an example using Python Flask.

**api_gateway.py:**

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

**Dockerfile:**

```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY . /app

RUN pip install flask requests

CMD ["python", "api_gateway.py"]
```

Build the Docker image for the API Gateway:

```bash
docker build -t api-gateway .
```

#### 3. **Push Images to Docker Registry**

Since Minikube doesn’t have direct access to local images by default, we can either push these images to a Docker registry (e.g., Docker Hub) or load them into Minikube using the following command:

```bash
minikube docker-env
eval $(minikube -p minikube docker-env)
```

Then build the Docker images as before.

#### 4. **Create Kubernetes Manifests**

Now, let’s define Kubernetes manifests for both services.

##### 4.1 **Backend Service Deployment**

**backend-service.yaml:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend-service
spec:
  replicas: 1
  selector:
    matchLabels:
      app: backend-service
  template:
    metadata:
      labels:
        app: backend-service
    spec:
      containers:
        - name: backend-service
          image: backend-service  # Use your image tag here
          ports:
            - containerPort: 5000
---
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  selector:
    app: backend-service
  ports:
    - port: 5000
      targetPort: 5000
  type: ClusterIP
```

##### 4.2 **API Gateway Deployment**

**api-gateway.yaml:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-gateway
spec:
  replicas: 1
  selector:
    matchLabels:
      app: api-gateway
  template:
    metadata:
      labels:
        app: api-gateway
    spec:
      containers:
        - name: api-gateway
          image: api-gateway  # Use your image tag here
          ports:
            - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: api-gateway
spec:
  selector:
    app: api-gateway
  ports:
    - port: 8080
      targetPort: 8080
  type: LoadBalancer
```

#### 5. **Deploy the Services to Kubernetes**

Now we can use `kubectl` to deploy these services to the Minikube cluster.

First, apply the backend service deployment:

```bash
kubectl apply -f backend-service.yaml
```

Then, apply the API Gateway deployment:

```bash
kubectl apply -f api-gateway.yaml
```

#### 6. **Accessing the Services**

Once the services are running, we need to check the external IP of the API Gateway service.

```bash
kubectl get services
```

Look for the `api-gateway` service and note its external IP. If you're using Minikube, you can also use the following command to open the API Gateway:

```bash
minikube service api-gateway
```

This should open the browser with the `API Gateway` service, and it should return the message from the backend service:

```
API Gateway: Hello from the Backend Service!
```

### 7. **Clean Up**

Once you're done, you can delete the deployed services:

```bash
kubectl delete -f api-gateway.yaml
kubectl delete -f backend-service.yaml
```

And stop Minikube:

```bash
minikube stop
```

---

This is a very basic example, but it should help you get started with microservices in Minikube and Kubernetes! Let me know if you need further assistance.