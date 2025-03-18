# Create a backery for Linux Base Image with Python 3.9 Pre installed

Here's a **Bakery Foundation** example that builds a **Linux machine image** with **Python 3.9** and uploads it to **Docker Hub** using `docker buildx`.  

---

## **Prerequisites**
- **Linux or macOS**
- **Docker installed** (`sudo apt install docker.io -y`)
- **Docker Buildx enabled** (`docker buildx version`)
- **A Docker Hub account** (Sign up at [hub.docker.com](https://hub.docker.com/))
- **Logged into Docker Hub** (`docker login`)

---

## **Step 1: Create a `Dockerfile`**
This **Dockerfile** will define a **base image** with **Python 3.9**.

```dockerfile
# Use Ubuntu as the base image
FROM ubuntu:20.04

# Set non-interactive mode for installation
ENV DEBIAN_FRONTEND=noninteractive

# Update and install Python 3.9
RUN apt-get update && apt-get install -y \
    python3.9 python3.9-venv python3.9-dev && \
    rm -rf /var/lib/apt/lists/*

# Set Python 3.9 as default
RUN ln -sf /usr/bin/python3.9 /usr/bin/python

# Verify Python installation
RUN python --version

# Default command
CMD ["python3"]
```

---

## **Step 2: Build and Push the Image Using Buildx**
Enable **Docker Buildx** and **multi-platform** support:

```sh
docker buildx create --use
docker buildx inspect --bootstrap
```

Now, build and **push** the image to Docker Hub:

```sh
docker buildx build --platform linux/amd64,linux/arm64 \
    -t your-dockerhub-username/python39-bakery:latest \
    --push .
```

Replace `your-dockerhub-username` with your **Docker Hub username**.

---

## **Step 3: Pull and Run the Image**
Once the image is **pushed**, you can pull and run it on any machine:

```sh
docker run --rm -it your-dockerhub-username/python39-bakery python --version
```

You should see output like:

```
Python 3.9.x
```

---

## **Conclusion**
This **Bakery Foundation** method ensures a **consistent** Python 3.9 environment, deployable across any system via Docker. 🚀

Would you like to **extend** this with additional tools (e.g., `pip`, `venv`, or `Jupyter`)?