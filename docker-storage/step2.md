# Docker Bind Mounts

Bind mounts have been around since the early days of Docker. 

Bind mounts have limited functionality compared to volumes. They are not Docker objects.

When you use a bind mount, a file or directory on the host machine is mounted into a container. The file or directory is referenced by its full or relative path on the host machine. 

The file or directory does not need to exist on the Docker host already. It is created on demand if it does not yet exist. 

Bind mounts are very performant, but they rely on the host machine’s filesystem having a specific directory structure available. 

If you are developing new Docker applications, consider using named volumes instead. You can’t use Docker CLI commands to directly manage bind mounts.

#### Bind mount example

With the mount option we are required to create the mount point upfront

`mkdir target`{{execute}}

Start a container with a bind mount

`docker run -d -it --rm --name devtest --mount type=bind,source="$(pwd)"/target,target=/app nginx:latest`{{execute}}

Using docker inspect devtest command to verify that the bind mount was created correctly.

`docker inspect devtest -f "{{json .Mounts}}" | jq`{{execute}}

`docker exec devtest df -h`{{execute}}

Stop the container:

`docker container stop devtest`{{execute}}

#### Docker tmpfs mounts

tmpfs mount is temporary, and only persisted in the host memory.

This is useful to temporarily store sensitive files that you don't want to persist in either the host or the container writable layer

Disadvantages of tmpfs mount
- you can't share tmpfs mounts between containers like in volumes and bind mounts.
- only available if you're running Docker on Linux

Run docker with tmpfs in container /app directory
`docker run -d -it --rm --name tmptest --mount type=tmpfs,destination=/app nginx:latest`{{execute}}

Inspect the container:
`docker inspect tmptest -f "{{json .Mounts}}" | jq`{{execute}}

`docker exec tmptest df -h`{{execute}}

Stop the container:

`docker container stop tmptest `{{execute}}
----
