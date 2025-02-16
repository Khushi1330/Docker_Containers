# Docker Container for Python file
To create a Python Docker image for a simple "Hello, World!" application, follow these steps:

### 1. **Create a Python Application**

Start by creating a basic Python script that prints "Hello, World!".

Create a file named `app.py` with the following content:

```python
# app.py
print("Hello, World!")
```

### 2. **Create a Dockerfile**

Next, create a Dockerfile that defines how the Python application should be containerized. The Dockerfile will specify which base image to use (in this case, a Python image), copy the Python script into the container, and run it.

Create a file named `Dockerfile` with the following content:

```dockerfile
# Use an official Python runtime as a parent image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Run the Python script
CMD ["python", "app.py"]
```

### 3. **Build the Docker Image**

Now, you need to build the Docker image using the `Dockerfile`. Make sure you're in the directory where the `Dockerfile` and `app.py` are located.

Open your terminal and run the following command:

```bash
docker build -t python-hello-world .
```

This will tell Docker to build an image and tag it as `python-hello-world`.

### 4. **Run the Docker Container**

Once the image is built, you can run it as a container using the following command:

```bash
docker run python-hello-world
```

When you run this command, the Python script inside the container will be executed, and you should see the following output:

```bash
Hello, World!
```

---

### Summary of Files

**`app.py`**:
```python
print("Hello, World!")
```

**`Dockerfile`**:
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY . /app
CMD ["python", "app.py"]
```

### Conclusion

With these steps, you've created a Docker image for a simple Python "Hello, World!" application. You can now use this container to run the Python script inside any environment where Docker is installed.