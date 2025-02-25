# Docker Commands & Errors

## 1. Identify the error in the following Docker command:
```sh
docker run -d --name=mycontainer -p 5000 flaskapp
```
- a) Missing -it flag
- b) Incorrect volume syntax
- c) Missing image name ✅
- d) No errors

**Answer:** The image name is missing. The correct command should be:
```sh
docker run -d --name=mycontainer -p 5000 myimage flaskapp
```

---

## 2. How do you check logs of a running container?
- a) `docker logs <container_id>` ✅
- b) `docker ps -logs`
- c) `docker inspect logs`
- d) `docker history <container_id>`

**Answer:** `docker logs <container_id>` retrieves the logs of a specific running container.

---

## 3. Which command lists all running Docker containers?
- a) `docker ps -a`
- b) `docker ps` ✅
- c) `docker images`
- d) `docker inspect`

**Answer:** `docker ps` shows only running containers, while `docker ps -a` lists all containers, including stopped ones.

---

## 4. Which command is used to stop all running containers?
- a) `docker stop all`
- b) `docker stop $(docker ps -q)` ✅
- c) `docker ps -a stop`
- d) `docker container kill all`

**Answer:** `docker stop $(docker ps -q)` stops all running containers by getting their container IDs.

---

## 5. How do you remove an image forcefully?
- a) `docker rmi -f <image_id>` ✅
- b) `docker rm <image_id>`
- c) `docker remove image <image_id>`
- d) `docker delete <image_id>`

**Answer:** `docker rmi -f <image_id>` forcefully removes an image, even if containers reference it.

---

## 6. What is the purpose of the `.dockerignore` file?
- a) Ignore files when building an image ✅
- b) Ignore certain containers when running `docker ps`
- c) Exclude network configurations
- d) Prevent accidental deletion of containers

**Answer:** `.dockerignore` prevents specified files from being included in a Docker image build.

---

## 7. Which of the following is a key difference between Docker and Virtual Machines?
- a) Docker uses hypervisors, whereas VMs use containers
- b) Docker has lower overhead compared to VMs ✅
- c) VMs share a common kernel like containers
- d) Docker does not support multi-tenancy

**Answer:** Docker is more lightweight because it shares the host OS kernel instead of using a hypervisor.

---

## 8. Which security feature is unique to Docker compared to VMs?
- a) Namespace isolation ✅
- b) Virtualized CPU management
- c) Hardware emulation
- d) Kernel-based network separation

**Answer:** Docker uses namespaces to isolate processes within containers.

---

## 9. Which of the following is NOT a valid Docker network type?
- a) Bridge
- b) Overlay
- c) Host
- d) VirtualBox ✅

**Answer:** VirtualBox is a VM software, not a Docker network type.

---

## 10. What is the default network mode for a Docker container?
- a) Host
- b) None
- c) Bridge ✅
- d) Overlay

**Answer:** The bridge network mode is the default for Docker containers.

---

## 11. Which of the following storage drivers is best suited for high-performance workloads?
- a) aufs
- b) overlay2
- c) devicemapper ✅
- d) zfs

**Answer:** `devicemapper` is optimized for high-performance workloads.

---

## 12. What is the purpose of the `docker network inspect` command?
- a) Display network statistics
- b) Check network configurations ✅
- c) Delete a network
- d) List available network drivers

**Answer:** `docker network inspect` shows details about a specific Docker network.

---

## 13. Which of the following is NOT a valid Docker storage driver?
- a) ext4 ✅
- b) overlay2
- c) aufs
- d) devicemapper

**Answer:** `ext4` is a file system, not a Docker storage driver.

---

## 14. What is the default storage driver used by Docker on Linux?
- a) aufs
- b) overlay2 ✅
- c) btrfs
- d) zfs

**Answer:** `overlay2` is the default storage driver for most modern Linux distributions.

---

## 15. What does the CMD instruction do in a Dockerfile?
- a) Specifies the default command to run ✅
- b) Sets an environment variable
- c) Defines a new network
- d) Assigns a user to the container

**Answer:** `CMD` provides the default command that runs inside the container.

---

## 16. What is a key advantage of using a multi-stage build in Docker?
- a) Reduces image size ✅
- b) Improves container security
- c) Provides automatic scaling
- d) Enhances network performance

**Answer:** Multi-stage builds allow you to copy only necessary files into the final image, making it smaller.

---

## 17. What does the HEALTHCHECK instruction do in a Dockerfile?
- a) Defines a probe to monitor container health ✅
- b) Checks available system memory
- c) Ensures high availability
- d) Automatically restarts a failed container

**Answer:** `HEALTHCHECK` allows Docker to check the health status of a running container.

---
## 18. Which of the following is a valid Docker restart policy?
- a) always ✅
- b) never
- c) manual
- d) on-retry

**Answer:** `always` ensures the container restarts if it stops, even after a reboot.

---

## 19. What is the purpose of the EXPOSE instruction in a Dockerfile?
- a) Open a port for external access ✅
- b) Bind a container to a specific host port
- c) Force a container to use a static IP
- d) Create a secure connection between containers

**Answer:** `EXPOSE` opens ports to allow network communication.

---

## 20. How do you assign a static IP to a container?
- a) Using `docker network create --subnet`
- b) Assigning an IP manually via `docker-compose.yml`
- c) Using `docker run --ip`
- d) All of the above ✅

**Answer:** All these methods allow you to assign a static IP.

---

## 21. What is the key purpose of an ARG instruction in a Dockerfile?
- a) Define build-time variables ✅
- b) Set runtime environment variables
- c) Expose application logs
- d) Define a network alias

**Answer:** `ARG` is used for defining build-time variables.

---

## 22. How do you update an existing running container with a new image version?
- a) `docker update <container_id>`
- b) `docker pull <image_name> && docker restart <container_id>`
- c) `docker stop <container_id> && docker rm <container_id> && docker run <image_name>` ✅
- d) `docker commit <container_id> new_image`

**Answer:** The correct method is to stop, remove, and restart the container with the new image.

---

## 23. Which command is used to remove unused Docker objects?
- a) `docker system clean`
- b) `docker system prune` ✅
- c) `docker delete unused`
- d) `docker purge`

**Answer:** `docker system prune` removes unused containers, networks, images, and build cache.

---

## 24. Which command removes a running container?
- a) `docker rm -f <container_id>` ✅
- b) `docker stop <container_id>`
- c) `docker delete container <container_id>`
- d) `docker remove -f <container_id>`

**Answer:** `docker rm -f <container_id>` forcefully removes a running container.

---

## 25. Which command is used to list all Docker networks?
- a) `docker network ls` ✅
- b) `docker ls network`
- c) `docker list network`
- d) `docker network list`

**Answer:** `docker network ls` lists all Docker networks.

---

## 26. What does the `docker exec` command do?
- a) Executes a command in a running container ✅
- b) Executes a command in a stopped container
- c) Executes a command on the host machine
- d) Executes a command in a Docker image

**Answer:** `docker exec` allows you to run commands inside a running container.

---

## 27. Which command is used to inspect a Docker container?
- a) `docker inspect <container_id>` ✅
- b) `docker info <container_id>`
- c) `docker logs <container_id>`
- d) `docker stats <container_id>`

**Answer:** `docker inspect` provides detailed information about a container.

---

## 28. What is the purpose of the `docker-compose up` command?
- a) Starts all services defined in a `docker-compose.yml` file ✅
- b) Stops all running containers
- c) Builds Docker images
- d) Removes unused Docker objects

**Answer:** `docker-compose up` starts all services defined in the `docker-compose.yml` file.

---

## 29. Which command is used to build a Docker image from a Dockerfile?
- a) `docker build -t <image_name> .` ✅
- b) `docker create -t <image_name> .`
- c) `docker run -t <image_name> .`
- d) `docker image build -t <image_name> .`

**Answer:** `docker build -t <image_name> .` builds an image from a Dockerfile.

---

## 30. What does the `docker-compose down` command do?
- a) Stops and removes all containers, networks, and volumes defined in `docker-compose.yml` ✅
- b) Stops all running containers
- c) Removes all Docker images
- d) Deletes the `docker-compose.yml` file

**Answer:** `docker-compose down` stops and removes all resources created by `docker-compose up`.

---

## 31. Which command is used to remove all stopped containers?
- a) `docker container prune` ✅
- b) `docker prune containers`
- c) `docker rm all`
- d) `docker delete stopped`

**Answer:** `docker container prune` removes all stopped containers.

---

## 32. What is the purpose of the `docker-compose logs` command?
- a) Displays logs for all services defined in `docker-compose.yml` ✅
- b) Displays logs for a specific container
- c) Displays logs for Docker Compose itself
- d) Displays logs for Docker images

**Answer:** `docker-compose logs` shows logs for all services in the `docker-compose.yml` file.

---

## 33. Which command is used to force remove a running container?
- a) `docker rm -f <container_id>` ✅
- b) `docker stop <container_id>`
- c) `docker delete container <container_id>`
- d) `docker remove -f <container_id>`

**Answer:** `docker rm -f <container_id>` forcefully removes a running container.

---

## 34. What does the `docker-compose build` command do?
- a) Builds or rebuilds services defined in `docker-compose.yml` ✅
- b) Builds Docker images from a Dockerfile
- c) Builds Docker networks
- d) Builds Docker volumes

**Answer:** `docker-compose build` builds or rebuilds services defined in the `docker-compose.yml` file.

---

## 35. Which command is used to list all Docker volumes?
- a) `docker volume ls` ✅
- b) `docker ls volume`
- c) `docker list volume`
- d) `docker volume list`

**Answer:** `docker volume ls` lists all Docker volumes.

---

## 36. What is the purpose of the `docker-compose restart` command?
- a) Restarts all services defined in `docker-compose.yml` ✅
- b) Restarts a specific container
- c) Restarts Docker Compose itself
- d) Restarts Docker images

**Answer:** `docker-compose restart` restarts all services in the `docker-compose.yml` file.

---

## 37. Which command is used to remove all unused Docker networks?
- a) `docker network prune` ✅
- b) `docker prune networks`
- c) `docker rm networks`
- d) `docker delete networks`

**Answer:** `docker network prune` removes all unused Docker networks.

---

## 38. What does the `docker-compose pull` command do?
- a) Pulls images for services defined in `docker-compose.yml` ✅
- b) Pulls all Docker images on the host
- c) Pulls Docker Compose updates
- d) Pulls Docker volumes

**Answer:** `docker-compose pull` pulls images for services defined in the `docker-compose.yml` file.

---

## 39. Which command is used to remove all unused Docker volumes?
- a) `docker volume prune` ✅
- b) `docker prune volumes`
- c) `docker rm volumes`
- d) `docker delete volumes`

**Answer:** `docker volume prune` removes all unused Docker volumes.

---

## 40. What is the purpose of the `docker-compose pause` command?
- a) Pauses all services defined in `docker-compose.yml` ✅
- b) Pauses a specific container
- c) Pauses Docker Compose itself
- d) Pauses Docker images

**Answer:** `docker-compose pause` pauses all services in the `docker-compose.yml` file.

---

## 41. Which command is used to remove all unused Docker images?
- a) `docker image prune` ✅
- b) `docker prune images`
- c) `docker rm images`
- d) `docker delete images`

**Answer:** `docker image prune` removes all unused Docker images.

---

## 42. What does the `docker-compose unpause` command do?
- a) Unpauses all services defined in `docker-compose.yml` ✅
- b) Unpauses a specific container
- c) Unpauses Docker Compose itself
- d) Unpauses Docker images

**Answer:** `docker-compose unpause` unpauses all services in the `docker-compose.yml` file.

---

## 43. Which command is used to remove all unused Docker objects?
- a) `docker system prune` ✅
- b) `docker prune system`
- c) `docker rm system`
- d) `docker delete system`

**Answer:** `docker system prune` removes all unused Docker objects.

---

## 44. What is the purpose of the `docker-compose config` command?
- a) Validates and displays the `docker-compose.yml` configuration ✅
- b) Configures Docker Compose settings
- c) Configures Docker networks
- d) Configures Docker volumes

**Answer:** `docker-compose config` validates and displays the `docker-compose.yml` configuration.

---

## 45. Which command is used to display real-time resource usage statistics for containers?
- a) `docker stats` ✅
- b) `docker usage`
- c) `docker resource`
- d) `docker monitor`

**Answer:** `docker stats` displays real-time resource usage statistics for containers.

---

## 46. What does the `docker-compose exec` command do?
- a) Executes a command in a running service container ✅
- b) Executes a command in a stopped container
- c) Executes a command on the host machine
- d) Executes a command in a Docker image

**Answer:** `docker-compose exec` runs commands in a running service container.

---

## 47. Which command is used to display the Docker version and API version?
- a) `docker version` ✅
- b) `docker --version`
- c) `docker info`
- d) `docker --info`

**Answer:** `docker version` displays the Docker version and API version.

---

## 48. What is the purpose of the `docker-compose scale` command?
- a) Scales the number of containers for a service ✅
- b) Scales Docker images
- c) Scales Docker networks
- d) Scales Docker volumes

**Answer:** `docker-compose scale` adjusts the number of containers for a service.

---

## 49. Which command is used to display the history of an image's layers?
- a) `docker history <image_name>` ✅
- b) `docker layers <image_name>`
- c) `docker inspect <image_name>`
- d) `docker image layers <image_name>`

**Answer:** `docker history` shows the history of an image's layers.

---

## 50. What does the `docker-compose kill` command do?
- a) Stops running containers by sending a SIGKILL signal ✅
- b) Deletes all containers
- c) Removes Docker images
- d) Deletes Docker networks

**Answer:** `docker-compose kill` stops running containers forcefully.

---

## 51. Which command is used to create a Docker network?
- a) `docker network create` ✅
- b) `docker create network`
- c) `docker network add`
- d) `docker add network`

**Answer:** `docker network create` creates a new Docker network.

---

## 52. What does the `docker network connect` command do?
- a) Connects a container to a network ✅
- b) Connects two networks
- c) Connects a container to a volume
- d) Connects a container to an image

**Answer:** `docker network connect` connects a container to a specified network.

---

## 53. Which command is used to disconnect a container from a network?
- a) `docker network disconnect` ✅
- b) `docker disconnect network`
- c) `docker network remove`
- d) `docker remove network`

**Answer:** `docker network disconnect` removes a container from a network.

---

## 54. What is the purpose of the `docker network inspect` command?
- a) Displays detailed information about a network ✅
- b) Displays network statistics
- c) Deletes a network
- d) Lists available network drivers

**Answer:** `docker network inspect` provides detailed information about a Docker network.

---

## 55. Which command is used to remove a Docker network?
- a) `docker network rm` ✅
- b) `docker rm network`
- c) `docker delete network`
- d) `docker remove network`

**Answer:** `docker network rm` removes a Docker network.

---

## 56. What does the `docker volume create` command do?
- a) Creates a new Docker volume ✅
- b) Creates a new Docker network
- c) Creates a new Docker image
- d) Creates a new Docker container

**Answer:** `docker volume create` creates a new Docker volume.

---

## 57. Which command is used to inspect a Docker volume?
- a) `docker volume inspect` ✅
- b) `docker inspect volume`
- c) `docker volume info`
- d) `docker info volume`

**Answer:** `docker volume inspect` provides detailed information about a Docker volume.

---

## 58. What is the purpose of the `docker volume prune` command?
- a) Removes all unused Docker volumes ✅
- b) Removes all Docker volumes
- c) Removes all Docker networks
- d) Removes all Docker images

**Answer:** `docker volume prune` removes all unused Docker volumes.

---

## 59. Which command is used to list all Docker images?
- a) `docker images` ✅
- b) `docker list images`
- c) `docker image ls`
- d) `docker ls images`

**Answer:** `docker images` lists all Docker images.

---

## 60. What does the `docker rmi` command do?
- a) Removes a Docker image ✅
- b) Removes a Docker container
- c) Removes a Docker network
- d) Removes a Docker volume

**Answer:** `docker rmi` removes a Docker image.

---

## 61. What is the purpose of the `docker tag` command?
- a) Tags an image with a new name or tag ✅
- b) Tags a container with a new name
- c) Tags a volume with a new name
- d) Tags a network with a new name

**Answer:** `docker tag` assigns a new name or tag to an image.

---

## 62. Which command is used to push an image to a Docker registry?
- a) `docker push` ✅
- b) `docker upload`
- c) `docker send`
- d) `docker publish`

**Answer:** `docker push` uploads an image to a Docker registry.

---

## 63. What does the `docker pull` command do?
- a) Downloads an image from a registry ✅
- b) Uploads an image to a registry
- c) Creates a container
- d) Deletes an image

**Answer:** `docker pull` downloads an image from a Docker registry.

---

## 64. Which command is used to search for Docker images in a registry?
- a) `docker search` ✅
- b) `docker find`
- c) `docker lookup`
- d) `docker query`

**Answer:** `docker search` searches for Docker images in a registry.

---

## 65. What is the purpose of the `docker history` command?
- a) Displays the history of an image's layers ✅
- b) Displays the history of a container
- c) Displays the history of a volume
- d) Displays the history of a network

**Answer:** `docker history` shows the layers and commands used to build an image.

---

## 66. What is the purpose of the `docker volume ls` command?
- a) Lists all Docker volumes ✅
- b) Lists all Docker networks
- c) Lists all Docker images
- d) Lists all Docker containers

**Answer:** `docker volume ls` lists all Docker volumes.

---

## 67. Which command is used to remove a Docker volume?
- a) `docker volume rm` ✅
- b) `docker rm volume`
- c) `docker delete volume`
- d) `docker remove volume`

**Answer:** `docker volume rm` removes a Docker volume.

---

## 68. What does the `docker volume inspect` command do?
- a) Displays detailed information about a volume ✅
- b) Displays detailed information about a container
- c) Displays detailed information about an image
- d) Displays detailed information about a network

**Answer:** `docker volume inspect` provides detailed information about a Docker volume.

---

## 69. Which command is used to remove all unused Docker volumes?
- a) `docker volume prune` ✅
- b) `docker prune volumes`
- c) `docker rm volumes`
- d) `docker delete volumes`

**Answer:** `docker volume prune` removes all unused Docker volumes.

---

## 70. What is the purpose of the `docker volume create` command?
- a) Creates a new Docker volume ✅
- b) Creates a new Docker network
- c) Creates a new Docker image
- d) Creates a new Docker container

**Answer:** `docker volume create` creates a new Docker volume.

---

## 71. What is the default Docker network driver?
- a) Bridge ✅
- b) Host
- c) Overlay
- d) None

**Answer:** The default network driver is `bridge`.

---

## 72. Which command is used to create a custom bridge network?
- a) `docker network create --driver bridge` ✅
- b) `docker create network --driver bridge`
- c) `docker network add --driver bridge`
- d) `docker add network --driver bridge`

**Answer:** `docker network create --driver bridge` creates a custom bridge network.

---

## 73. What is the purpose of the `docker network inspect` command?
- a) Displays detailed information about a network ✅
- b) Displays network statistics
- c) Deletes a network
- d) Lists available network drivers

**Answer:** `docker network inspect` provides detailed information about a Docker network.

---

## 74. Which command is used to remove a Docker network?
- a) `docker network rm` ✅
- b) `docker rm network`
- c) `docker delete network`
- d) `docker remove network`

**Answer:** `docker network rm` removes a Docker network.

---

## 75. What does the `docker network disconnect` command do?
- a) Disconnects a container from a network ✅
- b) Disconnects two networks
- c) Disconnects a container from a volume
- d) Disconnects a container from an image

**Answer:** `docker network disconnect` removes a container from a network.

---

## 76. What is the purpose of the `docker-compose up` command?
- a) Starts all services defined in `docker-compose.yml` ✅
- b) Stops all running containers
- c) Builds Docker images
- d) Removes unused Docker objects

**Answer:** `docker-compose up` starts all services defined in the `docker-compose.yml` file.

---

## 77. Which command is used to stop all services defined in `docker-compose.yml`?
- a) `docker-compose down` ✅
- b) `docker-compose stop`
- c) `docker-compose kill`
- d) `docker-compose rm`

**Answer:** `docker-compose down` stops and removes all services.

---

## 78. What does the `docker-compose logs` command do?
- a) Displays logs for all services defined in `docker-compose.yml` ✅
- b) Displays logs for a specific container
- c) Displays logs for Docker Compose itself
- d) Displays logs for Docker images

**Answer:** `docker-compose logs` shows logs for all services in the `docker-compose.yml` file.

---

## 79. Which command is used to rebuild Docker images in `docker-compose.yml`?
- a) `docker-compose build` ✅
- b) `docker-compose create`
- c) `docker-compose run`
- d) `docker-compose up`

**Answer:** `docker-compose build` rebuilds Docker images.

---

## 80. What is the purpose of the `docker-compose restart` command?
- a) Restarts all services defined in `docker-compose.yml` ✅
- b) Restarts a specific container
- c) Restarts Docker Compose itself
- d) Restarts Docker images

**Answer:** `docker-compose restart` restarts all services in the `docker-compose.yml` file.

---

## 81. What is the purpose of the `FROM` instruction in a Dockerfile?
- a) Specifies the base image ✅
- b) Specifies the default command
- c) Specifies environment variables
- d) Specifies the working directory

**Answer:** `FROM` defines the base image for the Dockerfile.

---

## 82. Which instruction is used to copy files into a Docker image?
- a) `COPY` ✅
- b) `ADD`
- c) `RUN`
- d) `CMD`

**Answer:** `COPY` copies files or directories into the image.

---

## 83. What does the `RUN` instruction do in a Dockerfile?
- a) Executes commands during the build process ✅
- b) Specifies the default command
- c) Sets environment variables
- d) Defines the working directory

**Answer:** `RUN` executes commands during the image build process.

---

## 84. Which instruction is used to set environment variables in a Dockerfile?
- a) `ENV` ✅
- b) `ARG`
- c) `RUN`
- d) `CMD`

**Answer:** `ENV` sets environment variables in the image.

---

## 85. What is the purpose of the `EXPOSE` instruction in a Dockerfile?
- a) Opens a port for external access ✅
- b) Binds a container to a specific host port
- c) Forces a container to use a static IP
- d) Creates a secure connection between containers

**Answer:** `EXPOSE` opens ports to allow network communication.

---

## 86. Which command is used to display real-time resource usage statistics for containers?
- a) `docker stats` ✅
- b) `docker usage`
- c) `docker resource`
- d) `docker monitor`

**Answer:** `docker stats` shows real-time CPU, memory, and network usage for containers.

---

## 87. What does the `docker top` command do?
- a) Displays running processes in a container ✅
- b) Displays the top layers of an image
- c) Displays the top containers by resource usage
- d) Displays the top networks by traffic

**Answer:** `docker top` shows the processes running inside a container.

---

## 88. Which command is used to display the disk usage of Docker objects?
- a) `docker system df` ✅
- b) `docker disk usage`
- c) `docker df`
- d) `docker usage`

**Answer:** `docker system df` shows disk usage by images, containers, and volumes.

---

## 89. What is the purpose of the `docker events` command?
- a) Displays real-time events from the Docker daemon ✅
- b) Displays historical events from Docker containers
- c) Displays events from Docker networks
- d) Displays events from Docker volumes

**Answer:** `docker events` streams real-time events from the Docker daemon.

---

## 90. Which command is used to display the Docker system information?
- a) `docker info` ✅
- b) `docker system info`
- c) `docker version`
- d) `docker --info`

**Answer:** `docker info` provides system-wide information about Docker.

---

## 91. What does the `docker diff` command do?
- a) Shows changes to files/directories in a container ✅
- b) Shows differences between two images
- c) Shows differences between two containers
- d) Shows differences between two volumes

**Answer:** `docker diff` displays changes made to a container's filesystem.

---

## 92. Which command is used to force remove a running container?
- a) `docker rm -f` ✅
- b) `docker rm --force`
- c) `docker delete -f`
- d) `docker remove --force`

**Answer:** `docker rm -f` forcefully removes a running container.

---

## 93. What is the purpose of the `docker cp` command?
- a) Copies files/folders between a container and the host ✅
- b) Copies files/folders between two containers
- c) Copies files/folders between two hosts
- d) Copies files/folders between two images

**Answer:** `docker cp` copies files or directories between a container and the host.

---

## 94. Which command is used to check the version of Docker installed?
- a) `docker --version` ✅
- b) `docker version`
- c) `docker info`
- d) `docker --info`

**Answer:** `docker --version` displays the installed Docker version.

---

## 95. What does the `docker login` command do?
- a) Logs into a Docker registry ✅
- b) Logs into a Docker container
- c) Logs into a Docker network
- d) Logs into a Docker volume

**Answer:** `docker login` authenticates you to a Docker registry.

---

## 96. What is the purpose of the `docker save` command?
- a) Saves an image to a tar archive ✅
- b) Saves a container to a tar archive
- c) Saves a volume to a tar archive
- d) Saves a network to a tar archive

**Answer:** `docker save` exports an image to a tar archive.

---

## 97. Which command is used to load an image from a tar archive?
- a) `docker load` ✅
- b) `docker import`
- c) `docker extract`
- d) `docker uncompress`

**Answer:** `docker load` imports an image from a tar archive.

---

## 98. What does the `docker import` command do?
- a) Creates an image from a tarball ✅
- b) Imports a container into an image
- c) Imports a volume into an image
- d) Imports a network into an image

**Answer:** `docker import` creates an image from a tarball.

---

## 99. Which command is used to display the history of an image?
- a) `docker history` ✅
- b) `docker layers`
- c) `docker inspect`
- d) `docker image layers`

**Answer:** `docker history` shows the layers and commands used to build an image.

---

## 100. What is the purpose of the `docker tag` command?
- a) Tags an image with a new name or tag ✅
- b) Tags a container with a new name
- c) Tags a volume with a new name
- d) Tags a network with a new name

**Answer:** `docker tag` assigns a new name or tag to an image.

---
## 101. What is the purpose of the `docker volume create` command?
- a) Creates a new Docker volume ✅
- b) Creates a new Docker network
- c) Creates a new Docker image
- d) Creates a new Docker container

**Answer:** `docker volume create` creates a new Docker volume.

---

## 102. Which command is used to inspect a Docker volume?
- a) `docker volume inspect` ✅
- b) `docker inspect volume`
- c) `docker volume info`
- d) `docker info volume`

**Answer:** `docker volume inspect` provides detailed information about a volume.

---

## 103. What does the `docker volume prune` command do?
- a) Removes all unused Docker volumes ✅
- b) Removes all Docker volumes
- c) Removes all Docker networks
- d) Removes all Docker images

**Answer:** `docker volume prune` removes all unused Docker volumes.

---

## 104. Which command is used to list all Docker volumes?
- a) `docker volume ls` ✅
- b) `docker ls volume`
- c) `docker list volume`
- d) `docker volume list`

**Answer:** `docker volume ls` lists all Docker volumes.

---

## 105. What is the purpose of the `docker volume rm` command?
- a) Removes a Docker volume ✅
- b) Removes a Docker network
- c) Removes a Docker image
- d) Removes a Docker container

**Answer:** `docker volume rm` removes a Docker volume.

---

## 106. What is the purpose of the `docker network create` command?
- a) Creates a new Docker network ✅
- b) Creates a new Docker volume
- c) Creates a new Docker image
- d) Creates a new Docker container

**Answer:** `docker network create` creates a new Docker network.

---

## 107. Which command is used to connect a container to a network?
- a) `docker network connect` ✅
- b) `docker connect network`
- c) `docker network attach`
- d) `docker attach network`

**Answer:** `docker network connect` connects a container to a network.

---

## 108. What does the `docker network disconnect` command do?
- a) Disconnects a container from a network ✅
- b) Disconnects two networks
- c) Disconnects a container from a volume
- d) Disconnects a container from an image

**Answer:** `docker network disconnect` removes a container from a network.

---

## 109. Which command is used to inspect a Docker network?
- a) `docker network inspect` ✅
- b) `docker inspect network`
- c) `docker network info`
- d) `docker info network`

**Answer:** `docker network inspect` provides detailed information about a network.

---

## 110. What is the purpose of the `docker network prune` command?
- a) Removes all unused Docker networks ✅
- b) Removes all Docker networks
- c) Removes all Docker volumes
- d) Removes all Docker images

**Answer:** `docker network prune` removes all unused Docker networks.

---

## 111. What is the purpose of the `docker-compose up` command?
- a) Starts all services defined in `docker-compose.yml` ✅
- b) Stops all running containers
- c) Builds Docker images
- d) Removes unused Docker objects

**Answer:** `docker-compose up` starts all services defined in the `docker-compose.yml` file.

---

## 112. Which command is used to stop all services defined in `docker-compose.yml`?
- a) `docker-compose down` ✅
- b) `docker-compose stop`
- c) `docker-compose kill`
- d) `docker-compose rm`

**Answer:** `docker-compose down` stops and removes all services.

---

## 113. What does the `docker-compose logs` command do?
- a) Displays logs for all services defined in `docker-compose.yml` ✅
- b) Displays logs for a specific container
- c) Displays logs for Docker Compose itself
- d) Displays logs for Docker images

**Answer:** `docker-compose logs` shows logs for all services in the `docker-compose.yml` file.

---

## 114. Which command is used to rebuild Docker images in `docker-compose.yml`?
- a) `docker-compose build` ✅
- b) `docker-compose create`
- c) `docker-compose run`
- d) `docker-compose up`

**Answer:** `docker-compose build` rebuilds Docker images.

---

## 115. What is the purpose of the `docker-compose restart` command?
- a) Restarts all services defined in `docker-compose.yml` ✅
- b) Restarts a specific container
- c) Restarts Docker Compose itself
- d) Restarts Docker images

**Answer:** `docker-compose restart` restarts all services in the `docker-compose.yml` file.
