
# BUILD
Build an image.

```
docker build -t getting-started .
---------------------------------
DockerFile
----------
# pull image with node 12 and alphine OS
FROM node:12-alphine
# install packages
RUN apk add --no-cache python2 g++ make
# set root directory
WORKDIR /app
# copy all content in source (.) and paste in target root (.) 
COPY . .
# install node dependencies from .json
RUN yarn install --production
# default command to run when a starting a container from this image
CMD ["node", "src/index.js"]
# port where the container would be running
EXPOSE 3000
```

- `-t`: image tag.
- `-f`: optional, if the Dockerfile has other name you have to specify with this flag.
- `.`: path where Docker should look for the Dockerfile, in this case is the current directory.

# LOGIN
Login to a Docker registry.

```
docker login [OPTIONS] [SERVER]
```
- `-u`: user.
- `-p`: password
- `[SERVER]`: if no server is specified, the default is defined by the daemon.

# PUSH
Push an image to a repository. First, create repository in your docker registry.

```
docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname

docker tag getting-started:latest dgarridoa/getting-started:latest
docker push dgarridoa/getting-started
```

# RUN
Create a container from an image. Pull the image if doesn't exist.

```
docker run [OPTIONS] IMAGE [COMMAND][ARG]

docker run -d -p 3000:3000 getting-started

docker run -it image ls path

# start a dev-mode container from an image (without using the Dockerfile)
docker run -dp 3000:3000 \
	-w /app -v "$(pwd):/app" \
	node:12-alpine \
	sh -c "yarn install && yarn run dev"

docker run -d \
	--network todo-app --network-alias mysql \
	-v todo-mysql-data:/var/lib/mysql \
	-e MYSQL_ROOT_PASSWORD=secret \
	-e MYSQL_DATABASE=todos \
	mysql:5.7

```

- `-d`: run the container in detached mode (in the background).
- `-p host_port:container_port`: map port 3000 from the host to port 3000 in the container.
- `-network`: network to attach the container.
- `-e`: environment variables.
- `-i`: interactive, keep stdin open if not attached.
- `-t`: allocate a pseudo tty (teletype-writer).
- `-w`: set working dir.
- `-v host_volume_name:guest_path`: mount host named volume in container. Can be populated with container contents. Create volum if not exist.
- `-v host_path:guest_path`: mount host path in container.
- `--name`: set container name.
- `--rm`: automatically remove the container when it exits.
- `IMAGE`: the image to use.
- `bash/sh -c "comand"`: execute a bash command in the container.

# EXEC
Execute a command in a container.

```
docker exec container_id command

docker exec -it container_id bash/sh

docker exec -it container-id mysql -u root -p
```

# IMAGES
Show images installed
```
docker images
```

# PS
Show containers running
```
docker ps
```
- `-a`: show all containers.
# STOP
Stop the container.

```
docker stop container_id
```

# REMOVE
Remove stopped container.

```
docker rm container_id
```
- `-f`: force remove when the container is running.


Remove image.
```
docker rmi image_name:tag
```

# KILL
Kill container by id.
```
docker kill container_id
```

# VOLUME
Create a named volume in the host.

```
docker volume create [VOLUME]
```
- `inspect [VOLUME]`: show volume metadata.

# LOGS
Watch logs.
```
docker logs -f container_id
```

# NETWORK
Create network.
```
docker network create network_name

docker run -it --network network_name nicolaka/netshoot
> dig network_alias

docker run -dp 3000:3000 \
   -w /app -v "$(pwd):/app" \
   --network todo-app \
   -e MYSQL_HOST=mysql \
   -e MYSQL_USER=root \
   -e MYSQL_PASSWORD=secret \
   -e MYSQL_DB=todos \
   node:12-alpine \
   sh -c "yarn install && yarn run dev"
```

# COMPOSE

Example of docker-compose.yml. 
```
version: "3.7"
services:
	# network_alias
	app:
    image: node:12-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 3000:3000
    working_dir: /app
		# can you use relatives path in bind mount
		volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos
	# second component
  mysql:
    image: mysql:5.7
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos
# volumes names to be created
volumes:
  todo-mysql-data:
```

```
version: '3.8'

services:
  web:
    build:
      context: .
    ports:
      - 8000:5000
    volumes:
      - ./:/app
  mysqldb:
    image: mysql
    ports:
      - 3306:3306
    environment:
      - MYSQL_ROOT_PASSWORD=p@ssw0rd1
    volumes:
      - mysql:/var/lib/mysql
      - mysql_config:/etc/mysql

volumes:
  mysql:
  mysql_config:
```

Run application stack specified in docker-compose.yml in detach mode.
```
docker compose up -d
```
By default, Docker Compose automatically creates a network specifically for the application stack. Network `app_default`, volume `app_todo-mysql-data`, creating `app_app_1` and `app_mysql_1`.

- `-f`: yml file.
- `--build`: create image if is not given.

Containers per application stack.
```
docker compose ps
```

Watch logs.
```
docker compose logs -f [service]
```

Tear it all down.
```
docker compose down
```
- `--volumes`: remove the volumes.

Run container.
```
docker compose run --rm myapp

```

# SCAN
Scan an image for security vulnerabilities.
```
docker scan image_name
```

# LAYERING
Show commands used to create each layer withing an image.
```
docker image history image_name
```
- `--no-trunc`: show full output.

Anytime a layer changes, all downstream layers have to be recreated as well. Putting the `COPY ..` after the installation of dependencies make faster the rebuild. 
```
 # syntax=docker/dockerfile:1
 FROM node:12-alpine
 WORKDIR /app
 COPY package.json yarn.lock ./
 RUN yarn install --production
 COPY . .
 CMD ["node", "src/index.js"]
```

Create a file named in the same folder as the Dockerfile. In his case, the `node_modules` folder should be omitted in the `COPY . .`.
```
.dockerignore
-------------
node_modules
```

