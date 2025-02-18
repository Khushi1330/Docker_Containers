# Docker Commands

Here is a detailed list of the **Docker commands** and their **flags**, including all the possible combinations and detailed explanations for each.

---

### **1. Docker Setup and Configuration**

- **`docker version`**  
  Displays Docker version information for both the client and server (Docker daemon).
  ```
  docker version
  ```

- **`docker info`**  
  Displays detailed information about Docker’s configuration and system status, including the number of containers, images, and Docker version.
  ```
  docker info
  ```

---

### **2. Image Management**

- **`docker pull <image_name>`**  
  Downloads an image from Docker Hub or a custom repository.
  ```
  docker pull <image_name>:<tag>
  ```
  - `tag`: Optional, defaults to `latest`. Specifies the image version.

- **`docker build -t <image_name>:<tag> <path>`**  
  Builds an image from a Dockerfile at the specified path.
  ```
  docker build -t <image_name>:<tag> <path>
  ```
  - `-t`: Tags the image with `<image_name>:<tag>`.
  - `<path>`: Path to the directory containing the `Dockerfile`.

- **`docker images`**  
  Lists all locally stored Docker images.
  ```
  docker images
  ```
  - `-a`, `--all`: Show all images, including intermediate images.
  - `-q`, `--quiet`: Show only image IDs.

- **`docker rmi <image_id>`**  
  Removes a Docker image from the local system.
  ```
  docker rmi <image_id>
  ```
  - `-f`, `--force`: Forces removal of the image, even if it’s being used by containers.

- **`docker tag <image_name>:<tag> <new_image_name>:<new_tag>`**  
  Tags an image with a new tag.
  ```
  docker tag <image_name>:<tag> <new_image_name>:<new_tag>
  ```

- **`docker history <image_name>`**  
  Displays the history of an image, including the layers and creation information.
  ```
  docker history <image_name>
  ```

- **`docker save -o <tar_filename> <image_name>`**  
  Saves a Docker image to a tarball file.
  ```
  docker save -o <tar_filename> <image_name>
  ```

- **`docker load -i <tar_filename>`**  
  Loads a Docker image from a tarball file.
  ```
  docker load -i <tar_filename>
  ```

---

### **3. Container Management**

- **`docker run <options> <image_name>`**  
  Creates and starts a container from an image.
  ```
  docker run [OPTIONS] <image_name>
  ```
  - `-d`, `--detach`: Runs the container in the background.
  - `-it`: Interactive terminal (combines `-i` and `-t`).
  - `--name <container_name>`: Assigns a custom name to the container.
  - `-p <host_port>:<container_port>`: Exposes and maps ports.
  - `-v <host_path>:<container_path>`: Mounts a volume.
  - `--rm`: Automatically removes the container when it stops.
  - `--env <key>=<value>`: Sets an environment variable inside the container.
  - `--restart <policy>`: Defines restart policy (`no`, `always`, `on-failure`).
  - `--network <network_name>`: Connects the container to a network.
  - `-e <key>=<value>`: Set an environment variable.
  - `--cpus <value>`: Limits the number of CPUs.

- **`docker ps`**  
  Lists all running containers.
  ```
  docker ps
  ```
  - `-a`, `--all`: Lists all containers, including stopped ones.
  - `-q`, `--quiet`: Displays only the container IDs.
  - `--filter`: Filters the list by conditions (e.g., status, name).

- **`docker exec -it <container_id> <command>`**  
  Runs a command inside a running container (interactive mode).
  ```
  docker exec -it <container_id> <command>
  ```
  - `-d`, `--detach`: Run the command in the background.
  - `--user <user>`: Specifies the user to run the command as.
  - `--env <key>=<value>`: Sets environment variables for the command.

- **`docker stop <container_id>`**  
  Stops a running container.
  ```
  docker stop <container_id>
  ```
  - `-t <seconds>`: Specifies the time to wait before forcibly stopping the container.

- **`docker start <container_id>`**  
  Starts a stopped container.
  ```
  docker start <container_id>
  ```

- **`docker restart <container_id>`**  
  Restarts a running or stopped container.
  ```
  docker restart <container_id>
  ```

- **`docker rm <container_id>`**  
  Removes a container.
  ```
  docker rm <container_id>
  ```
  - `-f`, `--force`: Forces removal of a running container.
  - `-v`, `--volumes`: Removes associated volumes.

- **`docker logs <container_id>`**  
  Fetches logs from a container.
  ```
  docker logs <container_id>
  ```
  - `-f`, `--follow`: Streams the logs.
  - `--tail <number>`: Shows the last N lines of logs.

- **`docker attach <container_id>`**  
  Attaches to a running container’s process.
  ```
  docker attach <container_id>
  ```

- **`docker pause <container_id>`**  
  Pauses a running container.
  ```
  docker pause <container_id>
  ```

- **`docker unpause <container_id>`**  
  Resumes a paused container.
  ```
  docker unpause <container_id>
  ```

- **`docker top <container_id>`**  
  Displays the running processes inside a container.
  ```
  docker top <container_id>
  ```

- **`docker stats`**  
  Displays real-time statistics for containers.
  ```
  docker stats
  ```

- **`docker inspect <container_or_image_id>`**  
  Shows detailed metadata about a container or image.
  ```
  docker inspect <container_or_image_id>
  ```

---

### **4. Network Management**

- **`docker network ls`**  
  Lists all Docker networks.
  ```
  docker network ls
  ```

- **`docker network create <network_name>`**  
  Creates a new Docker network.
  ```
  docker network create <network_name>
  ```
  - `--driver <driver>`: Specifies the network driver (e.g., `bridge`, `overlay`).

- **`docker network connect <network_name> <container_id>`**  
  Connects a container to a network.
  ```
  docker network connect <network_name> <container_id>
  ```

- **`docker network disconnect <network_name> <container_id>`**  
  Disconnects a container from a network.
  ```
  docker network disconnect <network_name> <container_id>
  ```

- **`docker network inspect <network_name>`**  
  Inspects a network and its configuration.
  ```
  docker network inspect <network_name>
  ```

---

### **5. Volume Management**

- **`docker volume ls`**  
  Lists all Docker volumes.
  ```
  docker volume ls
  ```

- **`docker volume create <volume_name>`**  
  Creates a new Docker volume.
  ```
  docker volume create <volume_name>
  ```

- **`docker volume inspect <volume_name>`**  
  Inspects a volume and displays details about it.
  ```
  docker volume inspect <volume_name>
  ```

- **`docker volume rm <volume_name>`**  
  Removes a Docker volume.
  ```
  docker volume rm <volume_name>
  ```

- **`docker volume prune`**  
  Removes all unused volumes.
  ```
  docker volume prune
  ```

---

### **6. Docker Compose (for Multi-Container Applications)**

- **`docker-compose up`**  
  Starts all containers defined in a `docker-compose.yml` file.
  ```
  docker-compose up
  ```
  - `-d`, `--detach`: Runs the containers in the background.
  - `--build`: Builds images before starting the containers.

- **`docker-compose down`**  
  Stops and removes containers defined in the `docker-compose.yml` file.
  ```
  docker-compose down
  ```

- **`docker-compose build`**  
  Builds or rebuilds services defined in the `docker-compose.yml` file.
  ```
  docker-compose build
  ```

- **`docker-compose logs`**  
  Shows logs for the services defined in the `docker-compose.yml`.
  ```
  docker-compose logs
  ```

- **`docker-compose ps`**  
  Lists the running containers for the defined services.
  ```
  docker-compose ps
  ```

- **`docker-compose exec <service_name> <command>`**  
  Executes a command inside a running container of a specified service.
  ```
  docker-compose exec <service_name> <command>
  ```

- **`docker-compose stop`**  
  Stops the services defined in `docker-compose.yml`.
  ```
  docker-compose stop
  ```

- **`docker-compose restart`**  
  Restarts the services defined in `docker-compose.yml`.
  ```
  docker-compose restart
  ```

- **`docker-compose config`**  
  Validates and prints the resolved `docker-compose.yml` configuration.
  ```
  docker-compose config
  ```

---

### **7. Docker Registry Management**

- **`docker login`**  
  Logs into a Docker registry (like Docker Hub).
  ```
  docker login
  ```

- **`docker logout`**  
  Logs out from the Docker registry.
  ```
  docker logout
  ```

- **`docker push <image_name>`**  
  Pushes an image to a Docker registry.
  ```
  docker push <image_name>
  ```

- **`docker search <term>`**  
  Searches for images in Docker Hub.
  ```
  docker search <term>
  ```

---

### **8. Docker System and Cleanup**

- **`docker system prune`**  
  Removes unused data such as stopped containers, unused images, networks, and volumes.
  ```
  docker system prune
  ```
  - `-a`, `--all`: Removes all unused images, not just dangling ones.
  - `-f`, `--force`: Skips the confirmation prompt.

- **`docker system df`**  
  Displays disk space usage by Docker images, containers, and volumes.
  ```
  docker system df
  ```

- **`docker volume prune`**  
  Removes all unused volumes.
  ```
  docker volume prune
  ```

- **`docker container prune`**  
  Removes all stopped containers.
  ```
  docker container prune
  ```

- **`docker image prune`**  
  Removes unused images (dangling images or images not associated with containers).
  ```
  docker image prune


  ```

- **`docker network prune`**  
  Removes unused networks.
  ```
  docker network prune
  ```

---

### **9. Docker Swarm (for Clustering)**

- **`docker swarm init`**  
  Initializes a Docker Swarm cluster.
  ```
  docker swarm init
  ```

- **`docker swarm join`**  
  Joins a node to a Docker Swarm cluster.
  ```
  docker swarm join
  ```

- **`docker node ls`**  
  Lists all nodes in the Docker Swarm.
  ```
  docker node ls
  ```

- **`docker service create`**  
  Creates a new service in a Docker Swarm.
  ```
  docker service create
  ```

- **`docker service ls`**  
  Lists all services in the Docker Swarm.
  ```
  docker service ls
  ```

- **`docker service update`**  
  Updates a service in a Docker Swarm.
  ```
  docker service update
  ```

- **`docker service scale`**  
  Scales a service in a Docker Swarm.
  ```
  docker service scale <service_name>=<replica_count>
  ```

- **`docker service rm`**  
  Removes a service from the Docker Swarm.
  ```
  docker service rm <service_name>
  ```

---

### **10. Other Docker Commands**

- **`docker events`**  
  Streams real-time events from Docker.
  ```
  docker events
  ```

- **`docker diff <container_id>`**  
  Shows the changes made to the filesystem of a container.
  ```
  docker diff <container_id>
  ```

- **`docker save`**  
  Saves a Docker image to a tar archive.
  ```
  docker save -o <file_name.tar> <image_name>
  ```

- **`docker load`**  
  Loads a Docker image from a tar archive.
  ```
  docker load -i <file_name.tar>
  ```

---

This should cover all major Docker commands and their possible combinations. Let me know if you need further explanations on any specific command!