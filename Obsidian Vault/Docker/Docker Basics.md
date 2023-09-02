Docker is a container technology: A tool for creating and managing containers. Container is a standardized unit of software A package of code and dependencies to run that code (e.g. NodeJS code + the NodeJS runtime). The same container always yields the exact same application and execution behavior! No matter where or by whom it might be executed.

Docker simplifies the creation and management of such containers!
###### Reasons:
We want to build and test in exactly (!) the same environment as we later run our app in.Every team member should have the exactly (!) same environment when working on the same project.When switching between projects, tools used in project A should not clash with tools used in project B.

Virtualization is also the same concept but it wastes a lot of space on your hard drive and tends to be slow. Reproducing on another computer/ server is possible but may still be tricky.

### STRUCTURE:
FROM node:14
WORKDIR /app
COPY package.json /app
RUN npm install
COPY . /app
EXPOSE 3000
CMD [ "node", "app.mjs" ]

Docker works on Layer Based Architecture or Approach. When the source code changed but dependencies remains same, we don't need to run "npm i" everytime. The repeatable steps are executed from the cache and only the changed files are executed by docker.
### COMMANDS:

	To create image -> docker build . 
	To run container with image -> docker run -p 3000(our local host):3000(port which container is exposed to) <image_hash>
	To list all containers -> docker ps -a
	To stop container -> docker stop <container-name>
	To start existing container -> docker start <container-name>
	for docker start -> Detached Mode (default) [container runnning in the background] which means the output is not listened and log is not printed in terminal
	for docker run -> Attached Mode (default) [container running in the foreground] which means output is listened by container and log is printed in the console.

	If you need interaction for the program, you can use -> docker run -it <image-hash>.
	If we restart with -> docker start <image-hash> it will be in default mode. If you need to be in interactive mode in attached mode, you can use [docker start -a -i <image-hash>] 

	To remove the container -> docker rm <container-name> [you can't remove the running container]
	To remove the image -> docker rmi <image-hash> [you can't remove the image that is being used by running or stopped container. You should remove that containers 1st and then you able to remove the image]
	To list all images -> docker images

	To remove the container when it's stopped -> docker run -p 3000:3000 -d --rm <image-hash>

	We can be able to copy the files/folders from the local to container and vice versa by using cp command. -> 
	docker cp Dummy/. (files in local) <container-name>:/ContainerDummy (will create new if it's not present in container) 
	docker cp <container-name>:/ContainerDummy Dummy (will get the Container folder into local machine)

	To name the container -> docker run -p 3000:3000 --name <any-name> <image-hash/image-name>
	To name the image   -> docker build -t <any-name>:<any-tag> .
	Name - Any name you can give. Tag - any specialized version for your custom image

	To clone image with other name -> docker tag <existing-image> <new-image-name> [this won't delete but create a copy of existing]

	To share your image into remote -> docker push <username/image-repo> (image name should me matched with repo you created in docker hub)[ you should sign in with docker login (only for push) ]
	To pull the custom image -> docker pull <username/image-repo> [you don't have to be logged in since you've chosen public in docker hub]

	We can detach from attached mode by
	docker run -p 8000:3000 -d <image-hash>
	We can attach from detached mode by
	docker attach <container-name>

### Volumes

Volumes are folders on your host machine hard drive which are mounted (“made available”, mapped) into containers. Volumes persist if a container shuts down. If a container (re-)starts and mounts a volume, any data inside of that volume is available in the container. A container can write data into a volume and read data from it.

There are two types of External Data Storages :
Volumes (Anonymous & Named) -
		*Named is used often so that it won't delete even if we removed container
		Docker sets up a folder / path on your host machine, exact location is unknown to you (= dev). Managed via docker volume commands.
		Great for data which should be persistent but which you don’t need to edit directly.

Bind Mounts -
		*You define a folder / path on your host machine
		Great for persistent, editable (by you) data (e.g. source code).


In Docker, there are three primary ways to manage data between the host system and containers: bind mounts, named volumes, and anonymous volumes. While they serve similar purposes, they have distinct use cases and characteristics.

1. **Bind Mount:**
   A bind mount maps a specific directory or file on the host system directly into a container. The data is shared between the host and container, and changes are immediately reflected on both sides. Bind mounts provide great flexibility and direct access to the host filesystem.

   Example:
   ```bash
   docker run -v /host/path:/container/path my-image
   ```

2. **Named Volume:**
   A named volume is a persistent data storage solution managed by Docker itself. It creates a special directory within Docker's storage system that is not directly accessible from the host system. Named volumes are independent of the container lifecycle, meaning they can be easily reused between containers. They are recommended for scenarios where you want to separate data storage from container management.

   Example:
   ```bash
   docker run -v my-volume:/container/path my-image
   ```

3. **Anonymous Volume:**
   An anonymous volume is similar to a named volume, but it is automatically created by Docker without a specific name. It is mainly used for temporary data storage within containers. Anonymous volumes are also independent of the container lifecycle and are typically used for storing data that doesn't need to be persisted long-term.

   Example:
   ```bash
   docker run -v /container/path my-image
   ```

Bind mounts offer more direct control over data and are often used during development for easy code sharing. Volumes (both named and anonymous) are managed by Docker and are generally preferred for production use due to their managed nature and portability.

In summary, bind mounts and volumes in Docker provide different ways to manage data between the host system and containers. Bind mounts give you direct access to host filesystem directories, while volumes offer a more abstracted and managed approach to data storage, suitable for various use cases.


### COMMANDS:

	docker run -p 3000:3000 -d --rm -v <volume-name>:<container-path> (eg., /app/feedback) -v "" --name <container-name> <image-name>

	docker run -d -p 3000:3000 --rm --name feedback-app -v feedback:/app/feedback -v ${pwd}:/app -v /app/node_modules feedback:volume

Let's break down the command step by step:

```bash
docker run -d -p 3000:3000 --rm --name feedback-app -v feedback:/app/feedback -v ${pwd}:/app -v /app/node_modules feedback:volume
```

1. `docker run`: This is the command to start a new container from an image.

2. `-d`: This flag stands for "detached" mode, which means the container will run in the background.

3. `-p 3000:3000`: This flag maps port 3000 from the host machine to port 3000 in the container. It allows you to access the application inside the container through `http://localhost:3000` on your host machine.

4. `--rm`: This flag indicates that the container should be automatically removed when it's stopped. This is useful for cleanup to avoid accumulating stopped containers.

5. `--name feedback-app`: This flag sets the name of the container to "feedback-app".

6. `-v feedback:/app/feedback`: This creates a named volume named "feedback" and mounts it to the `/app/feedback` directory inside the container. This allows for persistent storage of data, such as feedback-related files.

7. `-v ${pwd}:/app`: This is binding the current working directory (`${pwd}` is the equivalent of `$PWD` in PowerShell) on the host machine to the `/app` directory inside the container. This can be used to provide the application code to the container or to share files between the host and container.

8. `-v /app/node_modules`: This binds the `/app/node_modules` directory within the container. This is often used to speed up development by utilizing cached Node.js modules from the host system rather than installing them a new in the container.

9. `feedback:volume`: This is the name of the Docker image to be used for running the container.

So, in summary, this command runs a Docker container named "feedback-app" based on the "feedback:volume" image. It maps port 3000, creates a named volume "feedback" for persistent storage, mounts the current directory from the host to `/app` in the container, and binds the `/app/node_modules` directory to optimize Node.js module caching. The `--rm` flag ensures that the container is removed after it's stopped.

	docker run -d -p 3000:3000 --rm --name feedback-app -v feedback:/app/feedback -v ${PWD}:/app:ro -v /app/node_modules feedback:volume

The "ro" specifies read only which means the container has only the read access and it ensures we won't change files inside of our container.


#### Runtime Environment Variable

You can set env variable for port or any other in Dockerfile or in Docker CLI

ENV PORT 8000
EXPOSE ${PORT}

or

docker run -d -p 3000:3030 --env PORT=3030 -v feedback:/app/feedback --rm --name feedback-app -v ${pwd}:/app -v /app/node_modules -v /app/temp feedback:env

### Build Arguments

You can pass arguments if you came across variable values to be used in different images. For example you may need different version of base image for dev/prod image.

The important part here is that arguments can be used anywhere, not just into environment variables. It can be used whenever there's something in your Dockerfile that would be good to not have hardcoded.

Imagine for example you want to build your image for use with different NodeJS versions for whatever reason. Maybe you just want an easy test to see if anything breaks in the new version.

Now, instead of doing `FROM node:14`, you do `FROM node:$NODE_VERSION`. Now you can decide which version of NodeJS you use when you build the image, by passing an argument like `docker build -t feedback-node:node14 --build-arg NODE_VERSION=14 .`, instead of having to change your dockerfile all the time.

Another example: you might also have images that are basically the same, except for the content that gets copied. If you have multiple node applications that basically use the same image, you could use an argument to choose which directory gets copied into the image, so instead of having two entire Dockerfiles, you just have one.

These are just the first things that come to mind, I'm sure in more complex images, there's a lot more that would make sense to keep flexible. You have to remember that this is only a simple example we're building here, and using simple examples of how certain things work. Even if there is a different or even easier way to accomplish what this specific example does, you have to conentrate on the concept, rather than the specifics of the example.

**Command**
	docker build -t feedback:node12 --build-arg NODE_VERSION=12 .

**Dockerfile**
ARG NODE_VERSION=latest
FROM node:$NODE_VERSION
WORKDIR /app
COPY package.json /app
RUN npm i
COPY . /app
ENV PORT 8000
EXPOSE ${PORT}
CMD [ "npm","start" ]




