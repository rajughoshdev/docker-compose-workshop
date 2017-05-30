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
FROM node:6.2.0
RUN mkdir -p /var/www/node
ADD ./myapp/ /var/www/node
WORKDIR /var/www/node
CMD npm install
CMD npm start
EXPOSE 8082

```

We use the base image of `node:6.2.0`. It is the node official image of Docker.

In the `Dockerfile` we creates the directory `www/node` where our code will be store, then we added our project all code in `/var/www/node`. 
After that we run`npm install` and `npm start` to install all the packages and strar npm server. We used `8082` port for running our server.  


3. Review `docker-compose.yml`:

```
version: '2'
services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    ports:
      - "3309:3306"
    environment:
      MYSQL_ROOT_PASSWORD: mypassword

  node:
    links:
     - db
    build: .
    volumes:
      - "./myapp:/var/www/node"
    ports:
      - "8082:8082"

volumes:
    db_data:
  
```
Review application `config`:

```
{
  "development": {
	"defaultDbDriver": "mysql",
    "mysql": "mysql://root:mypassword@db/myapp"
  }
}

```

## Run the app

### Build the images

With docker-compose we can build all the images at once:
```
docker-compose build
```
The `docker-compose build` reads `docker-compose.yml` and build all the services defined in there.


### Start services

We can run `docker-compose up` that builds, (re)creates, starts, and attaches to containers for a service. 

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

If we want to review the logs of a specific service, e.g. `node`:
```
docker-compose logs node
```

### List containers

We can run `docker ps or docker-compose ps ` to list containers and their status:
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
docker-compose stop node
```

### Start container

Starts existing containers for a service, e.g. `node`:
```
docker-compose start node
```

### Run a command against a service
We can run a one-time command against a service. For example, the following command starts the `node` service and runs `bash` as its command.
```
docker-compose run web sh
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
### Remove volume 

We can run `docker volume rm < volume name>` to list volumes:
```
docker volume rm < volume name>
```
### network List 

We can run `docker network ls` to list volumes:
```
docker network ls
```
