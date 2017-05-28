# Docker compose workshop
Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a Compose file to configure your application’s services. Then, using a single command, you create and start all the services from your configuration. 

**Using Compose is basically a three-step process.**

- Define your app’s environment with a `Dockerfile` so it can be reproduced anywhere.

- Define the services that make up your app in `docker-compose.yml` so they can be run together in an isolated environment.

- Lastly, run `docker-compose up` and Compose will start and run your entire app.


**Compose lifecycle of your application:**

- Start, stop and rebuild services
- View the status of running services
- Stream the log output of running services
- Run a one-off command on a service



## Project's components

Review `Dockerfile`:

```
FROM node:7.7.0-alpine

```

We use the base image of `node:7.7.0-alpine`. It is the official image in Alpine, a minimal OS.

The `Dockerfile` then creates the directory where our code will be stored, `/code`, and it copies the `package.json` so it can install the node dependencies.

Afterwards it copies all the code we have in the host machine and runs the command that will keep the container running.

3. Review `docker-compose.yml`:

```
version: '2'
services:
  
```

The `docker-compose.yml` file describes the services that make your app. In this example those services are a web server and database. The compose file also describes which Docker images these services use, how they link together, any volumes they might need mounted inside the containers. Finally, the `docker-compose.yml` file describes which ports these services expose. See the docker-compose.yml [reference](https://docs.docker.com/compose/compose-file/) for more information on how this file works.


## Run the app

### Build the images

With docker-compose we can build all the images at once running:
```
docker-compose build
```

The `docker-compose build` reads `docker-compose.yml` and build all the services defined in there.

### Run a command against a service
We can run a one-time command against a service. For example, the following command starts the `web` service and runs `sh` as its command.
```
docker-compose run web sh
```

### Start services

We can run `docker-compose up` that builds, (re)creates, starts, and attaches to containers for a service. Unless they are already running, this command also starts any linked services.

Type in your terminal: 

```
docker-compose up
```

If we want, we can run the containers in background with `-d` flag:
```
docker-compose up -d
```
### Logs

We can see the log output from services running:
```
docker-compose logs
```

If we want to review the logs of a specific service, e.g. `web`:
```
docker-compose logs web
```

### List containers

We can run `ps` like in `docker ps` to list containers and their status:
```
docker-compose ps
```

### Stop containers

```
docker-compose stop 
```

Stops running containers without removing them. They can be started again with `docker-compose start`.

If we want we can stop only one container:
```
docker-compose stop web
```

### Start container

Starts existing containers for a service, e.g. `web`:
```
docker-compose start web
```

### Remove containers
```
docker-compose rm
```

If we want to stop and remove them:

```
docker-compose down
```


### volume List 

We can run `docker volume ls` to list volumes:
```
docker volume ls
```
###Remove volume 

We can run `docker volume rm < volume name>` to list volumes:
```
docker volume rm < volume name>
```
### network List 

We can run `docker network ls` to list volumes:
```
docker network ls
```