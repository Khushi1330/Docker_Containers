






## Q1: What is Docker (up to 2 marks)? Explain its architecture with a Diagram (4 marks)? What are the key components of Docker architecture (up to 4 marks)?

---

### **What is Docker? (2 Marks)**

Docker is an open-source platform used to automate the deployment, scaling, and management of applications inside lightweight, portable containers. These containers package all the dependencies (code, libraries, environment variables, configuration files) needed to run the application, ensuring consistency across development, testing, and production environments.

**Key advantages:**

* Portability: Runs consistently across different environments.
* Efficiency: Containers are lightweight and use system resources more efficiently than virtual machines.
* Speed: Faster start-up and deployment times compared to traditional VMs.

---

### **Docker Architecture (4 Marks)**

Docker follows a **client-server architecture**. The client communicates with the Docker daemon (server), which performs actions like building, running, and distributing containers. The architecture supports modularity, with components like registries and volumes.

#### **Diagram:**

![Docker Architecture](images/Docker_Architecture.png)

---

### **Explanation of the Architecture:**

1. **Docker Client**:

   * Provides a command-line interface (`docker` CLI) used by users to interact with the Docker daemon.
   * Sends commands like `docker build`, `docker run`, `docker pull` to the Docker daemon using REST APIs.

2. **Docker Daemon (`dockerd`)**:

   * Listens for Docker API requests.
   * Manages Docker objects (images, containers, volumes, networks).
   * Can communicate with other daemons to manage services in a Docker Swarm.

3. **Docker Images**:

   * Read-only templates used to create containers.
   * Can be built from a Dockerfile.
   * Pulled from Docker Hub or custom registries.

4. **Docker Containers**:

   * Runnable instances of Docker images.
   * Lightweight and isolated from the host system but share the OS kernel.
   * Have their own filesystem, networking, and process space.

5. **Docker Registries**:

   * Storage and distribution system for Docker images.
   * Default public registry is **Docker Hub**.
   * Private registries can be set up for enterprise use.

6. **Docker Objects**:

   * Includes containers, images, volumes, networks, and services.
   * Managed via the Docker CLI or API.

---

### **Key Components of Docker Architecture (4 Marks)**

| Component              | Description                                                                |
| ---------------------- | -------------------------------------------------------------------------- |
| **Docker Client**      | Acts as the user interface to Docker. Sends commands to the Docker daemon. |
| **Docker Daemon**      | Performs container operations. Listens to Docker API requests.             |
| **Docker Images**      | Blueprints for containers. Stored in registries.                           |
| **Docker Containers**  | Running instances of images. Lightweight and isolated.                     |
| **Docker Registries**  | Hosts Docker images. Docker Hub is the default.                            |
| **Volumes & Networks** | Support persistent storage and communication between containers.           |

---




## Q2: Explain the Docker Swarm architecture with a diagram (up to 5 marks). Explain the key components (up to 5 marks).

### Docker Swarm Architecture (5 Marks)

Docker Swarm is Docker’s native orchestration and clustering tool that turns a pool of Docker hosts into a single, virtual Docker engine. This enables easier scaling, load balancing, and service discovery.

Swarm follows a **manager-worker architecture** where tasks are distributed to nodes based on availability and resource needs. Communication between nodes is encrypted and secure.

#### **Diagram:**

![Docker Swarm Architecture](images/docker_swarn.png)

This diagram illustrates a typical Docker Swarm setup with one **Manager Node** and multiple **Worker Nodes**, all connected via an overlay network.

---

### Key Components of Docker Swarm (5 Marks)

1. **Manager Node**

   * Responsible for managing the Swarm.
   * Handles orchestration tasks like scheduling services, maintaining cluster state, and service scaling.
   * Uses Raft consensus for fault tolerance.
   * Can also run containers (optional).

2. **Worker Node**

   * Executes tasks assigned by the manager.
   * Does not participate in the orchestration decisions.
   * Can be added/removed from the swarm dynamically.

3. **Services**

   * High-level definitions of containers to be deployed across the swarm.
   * Consist of multiple **tasks** which are distributed across worker nodes.
   * Can be **replicated services** (multiple instances) or **global services** (one per node).

4. **Tasks**

   * The atomic unit in Swarm; represents a container and its execution command.
   * Tasks are assigned to worker nodes.

5. **Overlay Network**

   * Allows communication between services running on different nodes.
   * Each service can connect to one or more overlay networks for secure internal communication.

6. **Ingress Load Balancer**

   * Automatically distributes incoming traffic to available service replicas.
   * Supports internal DNS resolution so services can refer to each other by name.

7. **Docker CLI/REST API**

   * Interface for users to interact with the Swarm.
   * Used for creating services, scaling applications, or inspecting the Swarm state.

---

### Summary:

Docker Swarm provides a lightweight and native way to manage containers at scale. It supports service discovery, fault tolerance, secure communication, and load balancing—all using a declarative model that developers can manage using the standard Docker CLI.

Would you like a comparison between Swarm and Kubernetes next?

---

Certainly! Here's a **detailed comparison table** between **Docker Swarm**, **Kubernetes**, and **Apache Mesos** across **15 key dimensions**, suitable for a **10-mark question** in an academic or interview setting.

---

## Q8: Compare Docker Swarm, Kubernetes, and Apache Mesos. (10 Marks)

| Feature / Criteria                | Docker Swarm                           | Kubernetes                                    | Apache Mesos                                      |
| --------------------------------- | -------------------------------------- | --------------------------------------------- | ------------------------------------------------- |
| **1. Developed By**               | Docker Inc.                            | Originally by Google, now CNCF                | Apache Software Foundation                        |
| **2. Release Year**               | 2015                                   | 2014                                          | 2009                                              |
| **3. Architecture Type**          | Simple manager-worker                  | Master-node with etcd                         | Two-level (Mesos master and frameworks)           |
| **4. Installation Complexity**    | Very easy, native to Docker            | More complex, requires configuration          | Most complex setup among the three                |
| **5. Learning Curve**             | Beginner-friendly                      | Moderate to steep                             | Steep (meant for large-scale distributed systems) |
| **6. Load Balancing**             | Built-in with routing mesh             | Manual service configuration (via Ingress)    | Requires Marathon or other frameworks             |
| **7. Auto-scaling**               | Not supported natively                 | Natively supported                            | Supported via Marathon                            |
| **8. Service Discovery**          | Built-in DNS-based                     | kube-dns / CoreDNS                            | Needs external tools                              |
| **9. High Availability (HA)**     | Supported via manager node replication | Strong HA with multiple master nodes          | Supported, complex configuration                  |
| **10. Storage Management**        | Basic volume support                   | Advanced, with persistent volumes and CSI     | External plugins required                         |
| **11. Health Checks**             | Basic health checks                    | Extensive support (liveness/readiness probes) | Supported via Mesos or frameworks                 |
| **12. Community & Ecosystem**     | Smaller, part of Docker ecosystem      | Very large, active, and growing               | Moderate but niche community                      |
| **13. GUI / Dashboard**           | No native dashboard                    | Kubernetes Dashboard                          | Available via Mesosphere DC/OS                    |
| **14. Container Runtime Support** | Docker only                            | Containerd, CRI-O, Docker (deprecated)        | Docker, rkt, custom runtimes                      |
| **15. Use Cases**                 | Best for small to medium projects      | Ideal for large-scale, production-grade apps  | Designed for big data and multi-framework ops     |

---

### Summary:

* **Docker Swarm**: Best suited for smaller deployments and simpler orchestration needs. Easy to use and integrate with Docker-based workflows.
* **Kubernetes**: Industry standard for container orchestration. Offers powerful features for scalability, resilience, and automation.
* **Apache Mesos**: A more general-purpose distributed systems kernel. Used for managing both containerized and non-containerized workloads, especially in data center environments.




## Q9: Compare virtualization vs Docker containerization. Explain the idea of virutalization an application using Docker with an example dockerfile(upto 5 marks).marks)

Certainly! Below is a **detailed 10-mark answer** for comparing **virtualization vs Docker containerization**, and explaining **application virtualization using Docker** with a sample Dockerfile.

---

## Q9:

### **Compare between virtualization vs Docker containerization (Up to 5 Marks)**

| Feature               | Virtualization (VMs)                                           | Docker Containerization                                  |
| --------------------- | -------------------------------------------------------------- | -------------------------------------------------------- |
| **Technology**        | Hypervisor-based virtualization                                | OS-level virtualization                                  |
| **Isolation**         | Full hardware-level isolation with separate OS instances       | Process-level isolation sharing host OS kernel           |
| **Performance**       | Higher overhead due to guest OS and hypervisor                 | Near-native performance with minimal overhead            |
| **Boot Time**         | Slow (in minutes)                                              | Fast (in seconds)                                        |
| **Resource Usage**    | Heavier – each VM has its own OS and kernel                    | Lightweight – containers share the host OS               |
| **Portability**       | Less portable due to OS dependencies                           | Highly portable across any environment with Docker       |
| **Size**              | Large image sizes (several GBs)                                | Small image sizes (few MBs to hundreds of MBs)           |
| **Security**          | Better isolation, more secure in hostile environments          | Shared kernel – increased risk without proper sandboxing |
| **Use Cases**         | Legacy apps, OS-level virtualization, infrastructure emulation | Microservices, CI/CD pipelines, scalable deployments     |
| **Example Platforms** | VMware, VirtualBox, KVM                                        | Docker, Podman                                           |

> ✅ **Conclusion:**
> Containers are best suited for **agile, microservices-based applications** and offer **faster deployment**, while VMs are more appropriate for **full-stack isolation** and **multi-OS support**.

---

### **Explain the idea of virtualization an application using Docker with a Dockerfile example (Up to 5 Marks)**

Docker enables **application-level virtualization** by packaging applications and their dependencies into isolated containers. This means that you don’t need to install the application’s runtime, libraries, or even the programming language on the host system — everything is defined in the **Dockerfile** and built into the image.

---

#### ✅ Example: Virtualizing a Python Flask Application using Docker

**1. Application:** A simple Flask web server
**2. Dockerfile:**

```Dockerfile
# Base image with Python
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Copy application code into container
COPY . /app

# Install required Python packages
RUN pip install -r requirements.txt

# Expose port on which app runs
EXPOSE 5000

# Define the startup command
CMD ["python", "app.py"]
```

**3. requirements.txt:**

```
flask
```

**4. app.py (Minimal Flask App):**

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Dockerized Flask!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

---

### 🧩 How It Works:

* The base image provides Python (virtual environment base).
* All code and dependencies are copied into the container.
* On build, Docker installs required packages (`pip install`).
* On run, the container starts the Flask app just like it would on your local machine.

### 📦 Result:

You now have a **self-contained, isolated runtime** environment for your Flask application. This container can be run anywhere — development, testing, or production — without any host-specific changes.

---


Here's a detailed answer suitable for a **10-mark question** on **YAML files**, with examples and clarity on structure and real-world relevance:

---

## Q: **Explain what a YAML file and its key features (upto 2 marks). What are the valid entries that a YAML file can contain (upto 2 marks)? Give an example of a YAML file used in Docker Compose (upto 3 marks). List four other use cases of YAML other than Docker Compose (upto 3 marks).**

---

### ✅ What is a YAML File and its Key Features (Up to 2 Marks)

**YAML** stands for "**YAML Ain't Markup Language**". It is a human-readable data serialization standard often used for configuration files.

#### ✨ Key Features of YAML:

* **Readable**: Clean and easy to write (indented using spaces).
* **Lightweight**: Simpler than XML or JSON for configs.
* **Hierarchical**: Supports nested data structures (lists, dictionaries).
* **Language-independent**: Widely supported in many programming environments.

---

### ✅ Valid Entries a YAML File Can Contain (Up to 2 Marks)

A YAML file supports the following **data types and structures**:

| Entry Type           | Example                                       |
| -------------------- | --------------------------------------------- |
| **Scalars**          | `name: docker-app`                            |
| **Lists**            | `- frontend`<br>`- backend`                   |
| **Dictionaries**     | `image: nginx`<br>`ports: ["80:80"]`          |
| **Nested Maps**      | `services:`<br>  `web:`<br>    `image: nginx` |
| **Comments**         | `# This is a comment`                         |
| **Booleans & Nulls** | `enabled: true`<br>`value: null`              |

> ⚠ YAML relies on **spaces for indentation** (no tabs allowed).

---

### ✅ Example of a YAML File Used in Docker Compose (Up to 3 Marks)

```yaml
version: '3.8'

services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
  api:
    image: my-backend
    ports:
      - "5000:5000"
    depends_on:
      - db
  db:
    image: postgres:14
    environment:
      POSTGRES_PASSWORD: example
```

#### 🔍 What It Does:

* Defines **three services**: `web`, `api`, and `db`.
* Maps ports for external access.
* Uses `depends_on` to control startup order.
* Sets an environment variable for PostgreSQL.

---

### ✅ Four Other Use Cases of YAML (Up to 3 Marks)

| Use Case                      | Description                                                           |
| ----------------------------- | --------------------------------------------------------------------- |
| **Kubernetes Manifests**      | Used to define pods, deployments, services, volumes, etc.             |
| **GitHub Actions**            | YAML defines CI/CD workflows in `.github/workflows/*.yml`.            |
| **Ansible Playbooks**         | Infrastructure automation tasks are written in YAML for provisioning. |
| **CircleCI / GitLab CI/CD**   | YAML-based configuration for continuous integration pipelines.        |
| **CloudFormation (Optional)** | Though primarily JSON, AWS CloudFormation also supports YAML syntax.  |

---

### ✅ Conclusion:

YAML plays a central role in **cloud-native technologies**, **DevOps tools**, and **configuration-driven infrastructure**. Its simplicity and structure make it an ideal choice for modern application deployment and automation.


