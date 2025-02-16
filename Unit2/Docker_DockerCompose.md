# What is Docker Compose?

**Docker Compose** is a tool used to define and manage multi-container Docker applications. With Docker Compose, you can define the services, networks, and volumes needed for your application in a single file called `docker-compose.yml`. This file allows you to configure multiple Docker containers, making it easier to manage complex applications that involve more than one service (e.g., a database container, a backend, and a frontend).

Docker Compose allows you to:
- **Define multi-container applications** in a single configuration file.
- **Create, start, and manage containers** and their dependencies.
- **Ensure the correct order of container startup** (e.g., a database container should be up and running before the frontend starts).

### How Docker Compose Works

Docker Compose uses a configuration file (`docker-compose.yml`) to define:
1. **Services**: These are the individual containers that make up your application. For example, a web server, a database, or a message broker.
2. **Networks**: A network that enables communication between containers.
3. **Volumes**: A storage location that allows containers to share data persistently.

### Example: Docker Compose for a Database and Python Frontend App

In this example, we will create a simple multi-container application where:
- **Database**: We’ll use a PostgreSQL container as the backend database.
- **Frontend**: A Python-based web app using Flask (or any Python framework) as the frontend will connect to the database.

---

### 1. Docker Compose Configuration (`docker-compose.yml`)

Below is an example `docker-compose.yml` file that defines the database and Python app containers:

```yaml
version: '3.8'

services:
  db:
    image: postgres:latest     # Use the official PostgreSQL image from Docker Hub
    environment:
      POSTGRES_DB: mydb        # The name of the database
      POSTGRES_USER: user      # Database user
      POSTGRES_PASSWORD: password  # Database password
    volumes:
      - db_data:/var/lib/postgresql/data   # Volume to persist database data
    networks:
      - app-network

  web:
    build: ./frontend          # Build the frontend from the local Dockerfile in the 'frontend' directory
    ports:
      - "5000:5000"            # Map port 5000 of the container to port 5000 of the host machine
    depends_on:
      - db                     # Ensure the database is up before the web app starts
    environment:
      DATABASE_URI: postgres://user:password@db/mydb  # Database URI for the app to connect to
    networks:
      - app-network

volumes:
  db_data:                     # Volume for storing persistent data
    driver: local

networks:
  app-network:                 # Define a custom network to connect containers
    driver: bridge
```

### Explanation of the `docker-compose.yml` File

- **`version`**: Defines the version of Docker Compose being used (3.8 is a widely supported version).
- **`services`**:
  - **db (PostgreSQL)**:
    - `image`: Specifies the image to be used for the service, in this case, the official `postgres` image from Docker Hub.
    - `environment`: Specifies environment variables like the database name, user, and password.
    - `volumes`: Defines a volume to persist the database data so it is not lost when the container is stopped or restarted.
    - `networks`: Connects the container to a specific network (`app-network`).
  - **web (Python App)**:
    - `build`: The location of the `Dockerfile` for the frontend app (Python app). It will build the Docker image from the `frontend` directory.
    - `ports`: Exposes the app's port (5000 in this case) to the host machine.
    - `depends_on`: Ensures the `web` app waits for the `db` service to be up before starting.
    - `environment`: Specifies the connection URI to the database (in this case, PostgreSQL).
    - `networks`: Connects the `web` app to the same network as the `db`.

- **`volumes`**: Defines a named volume `db_data` to store PostgreSQL data persistently.

- **`networks`**: Defines a custom network `app-network` to ensure the containers can communicate with each other.

---

### 2. Dockerfile for the Python App (`frontend/Dockerfile`)

Now, let’s create the Dockerfile for the Python app (Flask, Django, or any framework you prefer).

**`frontend/Dockerfile`**:

```dockerfile
# Start from a base Python image
FROM python:3.9-slim

# Set the working directory inside the container
WORKDIR /app

# Copy the requirements.txt file into the container
COPY requirements.txt /app/

# Install the required dependencies
RUN pip install -r requirements.txt

# Copy the application code into the container
COPY . /app/

# Expose the application port (Flask typically runs on port 5000)
EXPOSE 5000

# Define the command to run the app (for example, using Flask)
CMD ["python", "app.py"]
```

### 3. Python App Example (`frontend/app.py`)

Here is a simple Python Flask application that connects to the PostgreSQL database:

**`frontend/app.py`**:

```python
import os
from flask import Flask
import psycopg2

app = Flask(__name__)

# Get the database connection details from the environment variable
DATABASE_URI = os.getenv('DATABASE_URI', 'postgres://user:password@db/mydb')

@app.route('/')
def hello_world():
    # Connect to the PostgreSQL database
    conn = psycopg2.connect(DATABASE_URI)
    cursor = conn.cursor()

    cursor.execute("SELECT 'Hello from Flask and PostgreSQL!'")
    result = cursor.fetchone()

    conn.close()

    return result[0]

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

This simple Flask app:
- Connects to the PostgreSQL database using the `psycopg2` library.
- Executes a query to check the connection.
- Displays a message: `'Hello from Flask and PostgreSQL!'`.

### 4. Python App Requirements (`frontend/requirements.txt`)

This file lists the required Python libraries:

**`frontend/requirements.txt`**:

```
Flask
psycopg2
```

### 5. Running the Application with Docker Compose

Now that we have everything set up, we can run the application using Docker Compose.

1. **Build the images**:
   From the directory containing your `docker-compose.yml`, run:

   ```bash
   docker-compose build
   ```

2. **Start the containers**:
   Run the following command to start the containers defined in `docker-compose.yml`:

   ```bash
   docker-compose up
   ```

   Docker Compose will pull the necessary images (PostgreSQL, Python), build the frontend container from the `Dockerfile`, and start both containers.

3. **Access the app**:
   Once everything is running, you can access the Python Flask app at:

   ```
   http://localhost:5000
   ```

   You should see the message: `'Hello from Flask and PostgreSQL!'`, indicating that the Flask app is connected to the PostgreSQL database.

---

### Summary of Benefits of Using Docker Compose

- **Multi-container management**: Easily manage applications with multiple services (e.g., frontend, backend, database) in a single configuration file.
- **Isolation**: Each service runs in its own container, ensuring that they are isolated from each other.
- **Reproducibility**: Docker Compose allows you to define the application stack once, and run it in any environment (local, test, production).
- **Simplicity**: Simplifies the setup and configuration of complex applications, ensuring all services can communicate effectively within a defined network.

Docker Compose is especially useful for development environments where you need to spin up several services with minimal setup and configuration.