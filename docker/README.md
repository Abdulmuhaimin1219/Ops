# Common Docker Commands

This is a short summary of the most commonly used Docker commands. If you're new to Docker or an experienced user, this quick reference covers essential commands for managing the Docker environment.

## Table of Contents
- [Show All Local Docker Images](#show-all-local-docker-images)
- [Removing Docker Images](#removing-docker-images)
- [Show All Containers](#show-all-containers)
- [Show Docker Container Logs](#show-docker-container-logs)
- [Get a Container Shell](#get-a-container-shell)
- [Stopping Containers](#stopping-containers)
- [Removing Containers](#removing-containers)
- [Find Container IP Address](#find-container-ip-address)
- [Copy Files to/from Docker Container](#copy-files-tofrom-docker-container)
- [Purging Unused Resources](#purging-unused-resources)
- [Monitor System Resource Utilization](#monitor-system-resource-utilization)
- [Tips for Using Docker](#tips-for-using-docker)

## Show All Local Docker Images
List all Docker images on your system, including intermediate and unused images:
```bash
docker images -a
```

## Removing Docker Images
Remove a specific Docker image by its ID:
```bash
docker rmi <image_id>
```
Force remove a specific image:
```bash
docker rmi -f <image_id>
```
Force remove all images:
```bash
docker rmi -f $(docker images -aq)
```

## Show All Containers
List all containers, including running and stopped ones:
```bash
docker ps -a
```

## Show Docker Container Logs
View logs for a specific container:
```bash
docker logs <container_id>
```

## Get a Container Shell
Access an interactive shell inside a running container:
```bash
docker exec -it <container_id> /bin/bash
```
If `/bin/bash` is unavailable, try:
```bash
docker exec -it <container_id> /bin/sh
```
Note: The available shell depends on the Docker image.

## Stopping Containers
Stop a running container:
```bash
docker stop <container_id>
```
Force stop a container (kill):
```bash
docker kill <container_id>
```

## Removing Containers
Remove a stopped container:
```bash
docker rm <container_id>
```
Force remove a container (running or stopped):
```bash
docker rm -f <container_id>
```
Force remove all containers:
```bash
docker rm -f $(docker ps -aq)
```

## Find Container IP Address
Retrieve the IP address of a container:
```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container_name_or_id>
```

## Copy Files to/from Docker Container
Copy a file from your local machine to a container:
```bash
docker cp <local_file> <container_name_or_id>:<remote_file>
```
Copy a file from a container to your local machine:
```bash
docker cp <container_name_or_id>:<remote_file> <local_file>
```

## Purging Unused Resources
Remove all unused or dangling images, containers, volumes, and networks:
```bash
docker system prune
```
Remove all stopped containers and unused images (not just dangling ones):
```bash
docker system prune -a
```

## Monitor System Resource Utilization
Monitor resource usage (CPU, memory, etc.) for running containers:
```bash
docker stats
```

## Tips for Using Docker
Here are some practical tips to enhance your Docker workflow:
- **Use Descriptive Container Names**: When running containers, use the `--name` flag to assign meaningful names (e.g., `docker run --name my-app -d my-image`). This makes it easier to manage containers instead of relying on random IDs.
- **Leverage Docker Compose for Multi-Container Apps**: For applications with multiple containers, use `docker-compose.yml` to define and manage services, networks, and volumes in a single file.
- **Clean Up Regularly**: Run `docker system prune` periodically to free up disk space by removing unused resources. Be cautious with `docker system prune -a`, as it removes all unused images.
- **Check Image Sizes**: Use `docker images` to monitor image sizes and optimize your Dockerfiles to reduce image bloat (e.g., use multi-stage builds or smaller base images like `alpine`).
- **Use `.dockerignore`**: Create a `.dockerignore` file to exclude unnecessary files (e.g., `.git`, `node_modules`) from being copied into images during builds, speeding up the process.
- **Tail Logs in Real-Time**: Use `docker logs -f <container_id>` to follow logs in real-time, which is helpful for debugging running containers.
- **Limit Resource Usage**: Use flags like `--memory` and `--cpus` when running containers to prevent any single container from consuming excessive system resources (e.g., `docker run --memory="512m" --cpus="0.5" my-image`).