Bind mounts have been around since the early days of Docker. Bind mounts have limited functionality compared to volumes. When you use a bind mount, a file or directory on the host machine is mounted into a container. The file or directory is referenced by its full or relative path on the host machine. By contrast, when you use a volume, a new directory is created within Docker’s storage directory on the host machine, and Docker manages that directory’s contents.

The file or directory does not need to exist on the Docker host already. It is created on demand if it does not yet exist. Bind mounts are very performant, but they rely on the host machine’s filesystem having a specific directory structure available. If you are developing new Docker applications, consider using named volumes instead. You can’t use Docker CLI commands to directly manage bind mounts.


#### Bind mount example

With the mount option we are required to create the mount point upfront

`mkdir target`

Start a container with a bind mount

`docker run -d -it --name devtest --mount type=bind,source="$(pwd)"/target,target=/app nginx:latest`{{execute}}

Using docker inspect devtest command to verify that the bind mount was created correctly.

`docker inspect devtest -f "{{json .Mounts}}" | jq`{{execute}}

Stop the container:

`docker container stop devtest`{{execute}}

`docker container rm devtest`{{execute}}
