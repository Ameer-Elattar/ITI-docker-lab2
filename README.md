# ITI-docker-lab2

## Task 1: Run a container using nginx image, and mount a directory from your host into the Docker container.
example: /home/samy/nginx:/home/nginx (bind mount)

### Steps
#### 1. Create Bind Mount Directory
```bash
docker run -d --publish 4444:80 -v C:\Users\Elattar\Desktop\docker:/usr/share/nginx/html nginx
```


## Task 2: Create 2 docker network (net-1 & net-2)

### Steps
#### 1. Create 2 docker network (net-1 & net-2)
```bash
docker network create net-1
docker network create net-2
```

#### 2. Run 2 new containers using nginx:alpine image, and attach the net-1 to them.
```bash
docker run -d --name nginx-net1 --net net-1 nginx:alpine
docker run -d --name nginx-net2 --network net-1 nginx:alpine
```

#### 3. Run another 1 new containers using nginx:alpine image, and attach the net-2 to them.
```bash
docker run -d --name nginx-net3 --net net-2 nginx:alpine
```

#### 4. Inspect the 3 containers to know their IPs and write them aside.
```bash
docker inspect -f '{{.NetworkSettings.Networks.net-1.IPAddress}}' nginx-net1
172.18.0.2

docker inspect -f '{{.NetworkSettings.Networks.net-1.IPAddress}}' nginx-net2
172.18.0.3

docker inspect -f '{{.NetworkSettings.Networks.net-2.IPAddress}}' nginx-net3
172.19.0.2
can use : docker inspect nginx-net1 | grep IPAddress
```

#### 5. Enter a container in the net-1 network and try to ping a container in the net-2 network (What do you notice?)
```bash
docker exec -it nginx-net1 sh 
ping 172.19.0.2
ping failed
```

#### 6. Enter a container in the net-1 network and try to ping the other container in the same network (What do you notice?)
```bash
docker exec -it nginx-net1 sh 
ping 172.18.0.3
ping successful and return response
```

## Task 3: Explain the difference between Docker volumes and Bind Mount.
### Docker Volumes
● It is stored within a directory on the Docker host.
● Managed by Docker and are isolated from the core functionality of the host machine.
● Can be mounted into multiple containers simultaneously.
● Deleting a container does not delete the volume.


### Docker Bind Mounts
● Bind mounts have limited functionality compared to volumes.
● The file or directory is referenced by its full path on the host machine.
● When you use a bind mount, a file or directory  on the host is mounted into the container, and allowing the container to access the host's filesystem.
● It is created on demand if it does not yet exist.
