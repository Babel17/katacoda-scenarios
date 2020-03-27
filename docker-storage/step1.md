### Using Docker Storage

Docker docs: https://docs.docker.com/storage/

Topics covered:
- What is a Docker Volume?
- How do I view the default volumes? [docker volume ls]
- How do I use the default volumes? [docker volume create/inspect/rm]
- What are the different types of Docker Volumes?
- How do I view detailed info on a volume [docker inspect]
----
# Introduction

Docker has three options for containers to store files in the host machine so that the files are persisted even after the container stops: 

- volumes
- bind mounts
- tmpfs

If you’re running Docker on Linux you can also use a tmpfs mount. If you’re running Docker on Windows you can also use a named pipe.

Be aware that volumes are the approach recommended by Docker.

Unlike a bind mount, you can create and manage volumes outside the scope of any container.

# Docker Volumes

Create a volume:

`docker volume create my-vol`{{execute}}

In above example we are using "volume" option where a new directory is created within Docker’s storage directory on the host machine, and Docker manages that directory’s contents (you can see where by inspecting Mountpoint in further step).


List volumes:

`docker volume ls`{{execute}}

Inspect a volume:

`docker volume inspect my-vol`{{execute}}

Remove a volume:

`docker volume rm my-vol`{{execute}}

Create a volume when starting a container:

`docker run -d --rm --name devtest --mount source=myvol2,target=/app nginx:latest`{{execute}}

Using docker inspect devtest to verify that the volume was created and mounted correctly. Look for the Mounts section:

`docker inspect devtest -f "{{json .Mounts}}"|jq`{{execute}}

This shows that the mount is a volume, it shows the correct source and destination, and that the mount is read-write.

`docker exec devtest df -h`{{execute}}

Stop the container and remove the volume.

`docker container stop devtest`{{execute}}

Note that the volume still exists even though the container has gone:

`docker volume ls`{{execute}}

`docker volume rm myvol2`{{execute}}

----
# Other Docker Volume Examples

For example, the following creates a tmpfs volume called tmpfsvol with a size of 100 megabyte and uid of 1000.

docker volume create --driver local \
    --opt type=tmpfs \
    --opt device=tmpfs \
    --opt o=size=100m,uid=1000 \
    tmpfsvol

Another example that uses btrfs:

docker volume create --driver local \
    --opt type=btrfs \
    --opt device=/dev/sda2 \
    btrfsvol

Another example that uses nfs to mount the /path/to/dir in rw mode from 192.168.1.1:

docker volume create --driver local \
    --opt type=nfs \
    --opt o=addr=192.168.1.1,rw \
    --opt device=:/path/to/dir \
    nfsvol
 
----
