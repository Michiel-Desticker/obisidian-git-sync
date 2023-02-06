# Docker overview

## Why do you need docker?

- Compatibility
- Long setup time
- Different Dev/Test/Prod environments

## What can it do?

With Docker, you can run each component in a seperate container with its own dependencies and libraries.

- Containerize Applications
- Run each service with its own dependies in seperate containers

## What are containers?

Completely isolated environments that all share the same OS kernel.

## Docker commands

run - start a container
```
docker run {name container}
```

ps - list containers
```
docker ps

# Use docker ps -a to see all previously stopped or exited containers
docker ps -a
```

stop - stop a container
```
docker stop {name or container id of the container}
```

rm - remove a container
```
docker rm {name container}
```

images - list images
```
docker images
```

rmi - remove images
```
# First, delete all dependent containers
docker rmi {image name}
```

pull - download an image
```
docker pull {image}
```

append a command
```
# Make it sleep for 5 seconds
docker run ubuntu sleep 5
```

exec - execute a command
```
# Execute a docker command on your docker container
# eg. print the contents of a file
docker exec distracted_mcclintock cat /etc/hosts
```