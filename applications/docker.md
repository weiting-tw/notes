# Docker

## Remove Docker Images, Containers, and Volumes

### Purging All Unused or Dangling Images, Containers, Volumes, and Networks

```sh
# all & force
docker system prune -af
```

### Remove one or more specific images

```sh
# images
docker images -a

# remove image
docker rmi Image Image
```

### Removing Containers

```sh
# containers
docker ps -a

# remove containers
docker rm ID_or_Name ID_or_Name
```

### Remove all exited containers

```sh
# exited containers
docker ps -a -f status=exited

# remove all exited containers
docker rm $(docker ps -a -f status=exited -q)
```

### Removing Volumes

```sh
# Volumes
docker volume ls

# remove
docker volume rm volume_name volume_name
```
