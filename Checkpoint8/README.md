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
1

### Question3: Run docker build -t getting-started . a few times and then run docker image ls to get the list of your images, why do you still have one image listed even though you have tried building the image many times?

<p>The reason there is still only one image listed is because the tag of the image is still the same. Running this command numerous times will produce the same result of building the same image with the same tag. If we were to change the tag, the image will be listed because we are building an image with a different tag.</p>


### Question4: What are -d and -p flags? What does each flag do? Start another git bah or wsl terminal and run docker run -p 1000:3000 getting-started in it. Notice that -d is missing. What is the output? Embed it in your submission. Explain why this happened.

<p>The -d flag is short for --detach. The -d flag essentially tells docker to run the container in the background. The -p flag is short for --publish. The -p flag maps the ports between the host and the container. If we run the command without the -d flag, it produces the following output:

**$ docker run -p 127.0.0.1:3000:3000 getting-started**
```
Using sqlite database at /etc/todos/todo.db
Listening on port 3000
```

2


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

3

### Question7: How long did it take to create the image after you updated the code? It is still shorter than the first time you did it, why?
<p>It took 9.6 seconds to recreate the image after updating the code. It is still shorter than the first time because the application dependencies are still stored. This speeds up the creation of the image.</p>

### Question8: What is the error message you get when you try to run the app container? Embed the error in your submission and explain why you get this error at all.
<p> The error message we get is a port bind error which is indicating to us that the port we are trying to run with is already allocated to another service/container. Only a single service/container is allowed to listen on a specific port at a given time. The error produced is:</p>

**$ docker run -dp 127.0.0.1:3000:3000 getting-started**

```
2ff4da6abc5ae4e8c40f3604872d51f6d7d2eda161b012b5ae1cec94d640ef40
docker: Error response from daemon: driver failed programming external connectivity on endpoint competent_mestorf (5fb6def03dccfbcbeead5379729dda4784126861fe7e1db964d295619f3643a5): Bind for 127.0.0.1:3000 failed: port is already allocated.
```

4

### Question9: Repeat all the step for app update for: <p className="text-center">No tasks to do for CSP451 yet! Add one above!</p> and embed a screenshot of your app in your submission.

5


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

6

### Question11: What is the difference between a container that is stopped vs one that is deleted? Does a stopped container continue to use resources from the underlying machine?

<p>The difference between a container that is stopped vs one that is deleted is that a container that is stopped can be started again, while a container that is deleted cannot be started again. A stopped container does not continue to use resources from the underlying machine.</p>




# Part B - Share the application: Follow the instructions and submit the screenshots.




# Part C - Persist the DB: Follow the instructions and embed outputs of the commands asked for.
# Part D - Use bind mounts: Follow the instructions and embed outputs of the commands asked for.
# Part E - Multi container apps: Follow the instructions and embed outputs of the commands asked for and answer all questions.
# Part F - Use Docker Compose: Follow the instructions and embed your compose.yaml file.


