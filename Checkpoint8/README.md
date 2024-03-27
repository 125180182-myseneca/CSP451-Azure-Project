# Checkpoint8 Submission

- **COURSE INFORMATION: CSP451**
- **STUDENT’S NAME: Thuan Le**
- **STUDENT'S NUMBER: 125180182**
- **GITHUB USER ID: 125180182-myseneca**
- **TEACHER’S NAME: Atoosa Nasiri**

### Table of Contents
1. [Part A - Containerize an application](#part-a---containerize-an-application-follow-the-instructions-and-answer-all-questions)
2. [Part B - Share the application](#part-b---share-the-application-follow-the-instructions-and-submit-the-screenshots)
3. [Part C - Persist the DB](#part-c---persist-the-db-follow-the-instructions-and-embed-outputs-of-the-commands-asked-for)
4. [Part D - Use bind mounts](#part-d---use-bind-mounts-follow-the-instructions-and-embed-outputs-of-the-commands-asked-for)
5. [Part E - Multi container apps](#part-e---multi-container-apps-follow-the-instructions-and-embed-outputs-of-the-commands-asked-for-and-answer-all-questions)
6. [Part F - Use Docker Compose](#part-f---use-docker-compose-follow-the-instructions-and-embed-your-composeyaml-file)


---
# Part A - Containerize an application: Follow the instructions and answer all questions.

### Question1: If you run docker build -t getting-started . for a second time, the build time will be different from the first time, why? Why the number of steps are also different? Explain your answers in detail.

<p> When we run `docker build -t getting-started .` for a second time, the difference from the first time is that it takes a shorter timeframe to build the image. The number of steps are the same except the dockerfile already exists from the first time we ran the command. In addition, because we ran the command the first time, the application dependencies are already downloaded. This speeds up the building of the container application.</p>

### Question2: What does the -t flag do? If you do not use it what is the error? embed the error in your answer.

<p> The -t flag sets the target build stage to build. In other words, it tells docker the name you want to set for the final image, and tells docker where the Dockerfile is. If we do not use the -t flag, it produces the following error: </p>

**$ docker build getting-started .**
```
ERROR: "docker buildx build" requires exactly 1 argument.
See 'docker buildx build --help'.

Usage:  docker buildx build [OPTIONS] PATH | URL | -

Start a build
```
![1](/Checkpoint8/Checkpoint8_images/1.png)

### Question3: Run docker build -t getting-started . a few times and then run docker image ls to get the list of your images, why do you still have one image listed even though you have tried building the image many times?

<p>The reason there is still only one image listed is because the tag of the image is still the same. Running this command numerous times will produce the same result of building the same image with the same tag. If we were to change the tag, the image will be listed because we are building an image with a different tag.</p>


### Question4: What are -d and -p flags? What does each flag do? Start another git bah or wsl terminal and run docker run -p 1000:3000 getting-started in it. Notice that -d is missing. What is the output? Embed it in your submission. Explain why this happened.

<p>The -d flag is short for --detach. The -d flag essentially tells docker to run the container in the background. The -p flag is short for --publish. The -p flag maps the ports between the host and the container. If we run the command without the -d flag, it produces the following output:

**$ docker run -p 127.0.0.1:3000:3000 getting-started**
```
Using sqlite database at /etc/todos/todo.db
Listening on port 3000
```

![2](/Checkpoint8/Checkpoint8_images/2.png)


<p>The reason why this happened is because it is running the container in the foreground. This means we no longer have access to the bash terminal as opposed to running the container with the -d flag which will run the container in the background, we will have access to the bash terminal.</p>

### Question5: The previous question has created a new container with your app running in it. Which port in localhost must be used to reach it?

<p>The port to access the container is port 3000. This is specified when we run the `docker run -p 127.0.0.1:3000:3000 getting-started` command which maps our host port 3000 and the container port 3000.</p>

### Question6: Run docker ps and embed the output in your answer. If you have completed previous questions, you should have at least two containers running in your system. What is their difference? Can you explain how and why this was necessary?

<p>The difference between the two containers are the names of the containers. This is necessary as having multiple containers of the same name can be confusing. They may be built with the same image, but they are separate instances of containers.</p>

**$ docker ps**

```
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS         PORTS                                NAMES
bad9e3e6815c   getting-started   "docker-entrypoint.s…"   2 minutes ago    Up 2 minutes   3000/tcp, 127.0.0.1:3001->3001/tcp   distracted_lehmann
fd27a8e08def   getting-started   "docker-entrypoint.s…"   11 minutes ago   Up 3 minutes   127.0.0.1:3000->3000/tcp             mystifying_taussig
```

![3](/Checkpoint8/Checkpoint8_images/3.png)

### Question7: How long did it take to create the image after you updated the code? It is still shorter than the first time you did it, why?
<p>It took 9.6 seconds to recreate the image after updating the code. It is still shorter than the first time because the application dependencies are still stored. This speeds up the creation of the image.</p>

### Question8: What is the error message you get when you try to run the app container? Embed the error in your submission and explain why you get this error at all.
<p> The error message we get is a port bind error which is indicating to us that the port we are trying to run with is already allocated to another service/container. Only a single service/container is allowed to listen on a specific port at a given time. The error produced is:</p>

**$ docker run -dp 127.0.0.1:3000:3000 getting-started**

```
2ff4da6abc5ae4e8c40f3604872d51f6d7d2eda161b012b5ae1cec94d640ef40
docker: Error response from daemon: driver failed programming external connectivity on endpoint competent_mestorf (5fb6def03dccfbcbeead5379729dda4784126861fe7e1db964d295619f3643a5): Bind for 127.0.0.1:3000 failed: port is already allocated.
```

![4](/Checkpoint8/Checkpoint8_images/4.png)

### Question9: Repeat all the step for app update for: <p className="text-center">No tasks to do for CSP451 yet! Add one above!</p> and embed a screenshot of your app in your submission.

![5](/Checkpoint8/Checkpoint8_images/5.png)


### Question10: When it is required to run 'docker stop ` before removing the contianer? Explain your answer by example.

<p>It is required to run `docker stop` before removing the container because the container is still running. The process is still being used and which means you cannot delete/remove the source. Attempting to do so will create an error:</p>

**$ docker ps**

```
CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS         PORTS                      NAMES
3340c942dd17   getting-started   "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   127.0.0.1:3000->3000/tcp   gracious_cohen
```

**$ docker rm 3340c942dd17**

```
Error response from daemon: cannot remove container "/gracious_cohen": container is running: stop the container before removing or force remove
```

![6](/Checkpoint8/Checkpoint8_images/6.png)

### Question11: What is the difference between a container that is stopped vs one that is deleted? Does a stopped container continue to use resources from the underlying machine?

<p>The difference between a container that is stopped vs one that is deleted is that a container that is stopped can be started again, while a container that is deleted cannot be started again. A stopped container does not continue to use resources from the underlying machine.</p>

---

# Part B - Share the application: Follow the instructions and submit the screenshots.

![7](/Checkpoint8/Checkpoint8_images/7.png)
![8](/Checkpoint8/Checkpoint8_images/8.png)
![9](/Checkpoint8/Checkpoint8_images/9.png)
![10](/Checkpoint8/Checkpoint8_images/10.png)
![11](/Checkpoint8/Checkpoint8_images/11.png)

# Part C - Persist the DB: Follow the instructions and embed outputs of the commands asked for.

**$ docker volume inspect todo-db**

```
[
    {
        "CreatedAt": "2024-03-27T16:53:07Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/todo-db/_data",
        "Name": "todo-db",
        "Options": null,
        "Scope": "local"
    }
]
```

# Part D - Use bind mounts: Follow the instructions and embed outputs of the commands asked for.

**$ docker ps**

```
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS         PORTS                      NAMES
a0ec55523e3c   node:18-alpine   "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   127.0.0.1:3000->3000/tcp   xenodochial_curie
```

**$ docker logs a0ec55523e3c**
```
yarn install v1.22.19
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
Done in 28.19s.
yarn run v1.22.19
$ nodemon -L src/index.js
[nodemon] 2.0.20
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node src/index.js`
Using sqlite database at /etc/todos/todo.db
Listening on port 3000
[nodemon] restarting due to changes...
[nodemon] starting `node src/index.js`
[nodemon] restarting due to changes...
[nodemon] starting `node src/index.js`
[nodemon] restarting due to changes...
[nodemon] starting `node src/index.js`
[nodemon] restarting due to changes...
[nodemon] starting `node src/index.js`
Using sqlite database at /etc/todos/todo.db
Listening on port 3000
```


# Part E - Multi container apps: Follow the instructions and embed outputs of the commands asked for and answer all questions.

**mysql> SHOW DATABASES;**
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| todos              |
+--------------------+
5 rows in set (0.01 sec)
```

**dig mysql**

```
; <<>> DiG 9.18.21 <<>> mysql
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 50445
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;mysql.                         IN      A

;; ANSWER SECTION:
mysql.                  600     IN      A       172.21.0.2

;; Query time: 220 msec
;; SERVER: 127.0.0.11#53(127.0.0.11) (UDP)
;; WHEN: Wed Mar 27 17:26:11 UTC 2024
;; MSG SIZE  rcvd: 44
```

**mysql> select * from todo_items;**

```
+--------------------------------------+-------------------------+-----------+
| id                                   | name                    | completed |
+--------------------------------------+-------------------------+-----------+
| 7fefe91d-ce6a-4cfd-9770-5d6b9c4d4104 | This is for CSP451      |         0 |
| 941e91ad-1a78-47ea-8e01-c9839afec262 | Taught by Atoosa Nasiri |         0 |
| 03bcf65b-eaf4-441d-94a9-ffb2df3b7358 | tle53                   |         0 |
| aefcd8c3-4806-4280-8067-de8106396a78 | 125180182               |         0 |
+--------------------------------------+-------------------------+-----------+
4 rows in set (0.00 sec)
```


### Question1: Why do you need to create a network for working with sql server?
<p>You need to create a network for working with SQL server in order for the application to communicate to SQL server. Both containers are attached to the network to be able to communicate.</p>

### Question2: What happens if you need to kill the container that is running your sql server, do you need to create another network?
<p>You do not need to create another network. If you kill the container that is running your SQL server, you can create another container and attach it to the existing network.</p>

### Question3: Do an experiment: get the details of sql container using the inspect command and direct it to a file for now. Delete this container, create another one, and run the inspect command again. Compare and find the differences in network configuration/settings between the current and previous controller. Explain the differences, if any.
<p>Majority of the settings are still the same. There are small differences though such as the DNSNames, EndpointID, and the Hostname.</p>

# Part F - Use Docker Compose: Follow the instructions and embed your compose.yaml file.

[compose.yaml](/Checkpoint8/Checkpoint8_images/compose.yaml)

```
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 127.0.0.1:3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:

```
