### Using Docker Storage

Docker docs: https://docs.docker.com/storage/

Topics covered:
- What is a Docker Volume?
- How do I view the default volumes? [docker volume ls]
- How do I use the default volumes? [docker volume create/inspect/rm]
- What are the different types of Docker Volumes?
- How do I view detailed info on a volume [docker inspect]


#### TODO: add in the pictures 


__Docker has two options for containers to store files in the host machine, so that the files are persisted even after the container stops: volumes, and bind mounts. If you’re running Docker on Linux you can also use a tmpfs mount. If you’re running Docker on Windows you can also use a named pipe.__


Unlike a bind mount, you can create and manage volumes outside the scope of any container.

Create a volume:

`docker volume create my-vol`{{execute}}

List volumes:

`docker volume ls`{{execute}}

Inspect a volume:

`docker volume inspect my-vol`{{execute}}

Remove a volume:

`docker volume rm my-vol`{{execute}}

`docker run -d --name devtest --mount source=myvol2,target=/app nginx:latest`{{execute}}

Using docker inspect devtest to verify that the volume was created and mounted correctly. Look for the Mounts section:

`docker inspect devtest -f "{{json .Mounts}}"|jq`{{execute}}


This shows that the mount is a volume, it shows the correct source and destination, and that the mount is read-write.

Stop the container and remove the volume. Note volume removal is a separate step.

`docker container stop devtest`{{execute}}

`docker container rm devtest`{{execute}}

`docker volume rm myvol2`{{execute}}
 