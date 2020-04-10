# Shipping Docker

## The docker commands

We explore the three commands available to us after installing Docker on our computer:

* `docker`: the main command, manage the life cycle of docker containers
* `docker-compose`: coordinate the use of multiple containers (services) using a yaml file
* `docker-machine`: spin up servers, install Docker, and let us control them remotely

## Your first container

We inspected our Docker setup:

```
docker ps # Check for running containers:
docker ps -a # Check for existing containers, whether running or not
docker images # View local images
```

When you start a container, you need to start the container from its image. So images are the first baseline thing to create a container. The tag `latest` represent the default tag of some image.

Let's run our first image. We run an image ubuntu, tagged 16.04. Since that's not on my machine locally

```
# Lists out the current working directory inside the container
docker run ubuntu:16.04 ls -lah
```

It gets downloaded from Docker Hub (specifically `https://hub.docker.com/_/ubuntu/`). This URL is for the “ubuntu official repository”. You can identify official repositories of images when the URL has `/_/`

This container runs the `ls -lah` container and then stops. Since the ls command is short-running (runs, then exits), the container will stop once the ls command finishes. **Containers will only run as long as there's a process to run/monitor**. The container typically only runs one process.

We can see this container still exists but no longer is running:

```
docker ps # No output
docker ps -a # Shows our stopped container
CONTAINER ID    IMAGE           COMMAND    CREATED        STATUS               NAMES
52a27a2dfd75    ubuntu:16.04    "ls -lah"  13 hours ago   Exited (0) 2s ago    condescending_lamarr
```

## Inspecting containers

We can inspect our containers to get some more information out of them, using the docker inspect command. The output is JSON.

```
# Outputs json, an array of containers
docker inspect container_name_or_id
[{
    "Id": "abcdefghijklmnopqrstuvwxyz",
    ...
}]
```

We can get some specific information out of this in two ways. One way has no dependencies and works with the docker inspect command. It uses Golang-style template tags:

```
# Get main IP address
docker inspect --format="{{.NetworkSettings.IPAddress}}" container_name_or_id

# Get IP address of all connected networks
docker inspect --format="{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" container_name_or_id
```

Another way I parse this JSON output is with the `jq` utility. It has nicer output and easier formatting to parse JSON output.

```
# "Prettify" the json output:
docker inspect container_name_or_id | jq

# Parse the output, get main IP Address
docker inspect container_name_or_id | jq -r ".[0].NetworkSettings.IPAddress"

# Parse the output, get "bridge" network IP Address (same as above)
docker inspect container_name_or_id | jq -r ".[0].NetworkSettings.Networks.bridge.IPAddress"
```

## Cleaning up

When running a lot of Docker commands, we often end up with a lot of cruft. Here's how to clean up after ourselves.

### Containers

First, I'll make some more containers. Since each docker run command creates a new container, we can re-run our previous command a few more times to create a few more:

```
# Create two more containers
docker run ubuntu:16.04 ls -lah
docker run ubuntu:16.04 ls -lah

# See 2-3 stopped containers listed
docker ps -a
```

We can remove a container (permanently):

```
docker rm container_name_or_id
```

Or we can remove all containers with this trick:

```
docker rm $(docker ps -aq)
```

The docker `ps -aq` command (with the additional `-q` flag) will list containers but only output their ID.

We can run containers that clean-up after themselves as well. Adding the `--rm` flag ensures the container deletes itself when it's done runnng (when it stops):

```
# Remove the container when done running, via the --rm flag
docker run --rm ubuntu:16.04 ls -lah

# We'll see no new container listed here
docker ps -a
Images
We can do the same for images:
# List images
docker images

# Remove a specific image by ID or Name
docker rmi image_id_or_name:tag_combo

# Remove all images
docker rmi $(docker images -q)
```

## Interacting with a container (bash)

Since we can run just about any command within a container, let's try to run Bash. Bash is an interactive shell, so in theory, if we spin up bash within a container, we should be able to interact with that container. This is exactly what we do when we turn on our Terminal or iTerm app.

```
# Run bash, but it exists immediately
docker run ubuntu:16.04 bash

# Container exists (success status code)
docker ps -a

CONTAINER ID    IMAGE           COMMAND    CREATED         STATUS               NAMES
5b56c2bf5dde    ubuntu:16.04    "bash"     13 hours ago    Exited (0) 1s ago    fervent_babbage
```

We need to tell Docker to make this interactive!

```
# Add the -i and -t flags
docker run -i -t ubuntu:16.04 bash

# Shorten that to just -it
docker run -it ubuntu:16.04 bash
```

We can run docker run `--help` to see what the `-i` and `-t` flags do:

* `-i`, `--interactive`: Keep STDIN open even if not attached. This means we can input commands into the running process by typing
* `-t`, `--tty`: Allocate a pseudo-TTY, letting us get output as if we're logged into the container

This will look like we're logged into the container! We can then run commands against it:

```
root@5660cf3be26f:/# pwd
/

root@5660cf3be26f:/# whoami
root

root@5660cf3be26f:/# which apt-get
/usr/bin/apt-get
```

Let's look at the running processes:

```
root@5660cf3be26f:/# ps aux

USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  1.3  0.1  18240  3280 ?        Ss   00:39   0:00 bash
root        12  0.0  0.1  34424  2864 ?        R+   00:39   0:00 ps aux
```

There's only two! We usually see a huge list inside of a regular server. The container, however, will only run one process (and any secondary processes that single process starts). Here we see also our ps aux process running as well, which gets us this output.

Note that bash is started as PID 1 as well - the first process, which on a regular server is usually the init system that boots everything else.

Now we can exit the container, which will stop bash and therefore also the container.

```
root@5660cf3be26f:/# exit

docker ps

docker ps -a

CONTAINER ID    IMAGE           COMMAND    CREATED    STATUS                   NAMES
5660cf3be26f    ubuntu:16.04    "bash"     13h ago    Exited (0) 17 minutes    fervent_babbage
```

We can actually restart this stopped container as well!

```
docker start 5660cf3be26f

docker ps
```

The container is running! However, we're not "attached" - there's no `-it` equivalent passed when we restarted the container.

We can get that back tho!

```
docker stop 5660cf3be26f
docker start -i 5660cf3be26f

root@5660cf3be26f:/# whoami
root
```

Note that when we re-started it, we didn't need to define an image or command. It already had that defined based on the docker run command used to create it. So, starting or stopping this existing container was much simpler than creating it - it remembers the settings and variables used when restarting it.

## Nginx & Sharing Ports

Let's see how we might install Nginx into a container. We'll first do it a super-manual way so you get an idea of how Docker containers are functioning.

First we'll spin up a new container and run Bash so we can interact with it:

```
docker run -it ubuntu:16.04 bash
```

Then we can run some commands to install and run Nginx:

```
# Install nginx
apt-get update
apt-get install -y nginx

# Nginx does not start automatically like it
# might on a regular Ubuntu server
# Start nginx manually:
nginx

# See that Nginx is running by listing
# out all running processes
ps aux
```

If we open a new tab in our terminal and attempt to curl request against the container, we'll see it won't work. We haven't made any way to connect to the container externally.

```
# From our host machine, not from within the container
curl localhost

# web request against the container's IP address
curl 171.17.0.2
```

Let's see how to do this. We'll exit the container, delete it, and re-try it, this time forwarding port 80 on our host machine into port 80 in the container.

```
# exit from container
exit

# remove container
docker rm container_id

# restart new container
docker run -it -p 80:80 ubuntu:16.04 bash
```

Then we can re-install nginx, start it, and try another curl request against it!

```
apt-get update && apt-get install -y nginx
nginx
```

From our host machine (my Mac in this case) we can curl request it on localhost:80, since we're forwarding my Mac's port 80 to port 80 in the container.

```
curl localhost
> Welcome to nginx! (and other html output)
```

So, that works! We can see the default nginx page in a curl request and in our browser.

## Sharing volumes

We want to use Nginx in a container in a way that lets us serve our own content.

To do that, we'll learn a new flag for the Docker run command, the `-v` flag. This stands for "volumes" and it lets us share volumes between our host machine and the container.

Let's see how that might work:

```
# Create a new directory and get into to
mkdir dockertest
cd dockertest

# Create a new index.html file to server
echo "Hello, Shipping Docker!" > index.html

# Run a new container that we'll use for Nginx
# Share the current directory with the container
docker run -it -p 80:80 -v ~/dockertest:/var/www/html ubuntu:16.04 bash
```

Then we can quickly install nginx and run it:

```
# Install Nginx
apt-get update && apt-get install -y nginx

# Verify the default config is looking in /var/www/html
cat /etc/nginx/sites-available/default

# Check /var/www/html, confirm
# it has our shared index.html
cd /var/www/html && cat index.html

# Run nginx
nginx
```

Then we can curl request it from our host machine!

```
curl localhost
> Hello, Shipping Docker!
```

We can edit the index.html file to verify changes are reflected as well. I added a few exclamation marks to the index.html file and immediately could see the page updated with that content in the browser and in a curl request.

### Committing changes

...

## The Dockerfile

We want to automate the process we saw in the last video. This lets us create reusable images without having to take manual steps (installing Nginx for example).

Furthermore, we'll also create a file that can be used by other developers to get the exact same results. Instead of sharing large images from a centralized repository (such as Docker Hub), we can share a file and have someone else build it.

Inside of a new nginx directory, we create a new file called Dockerfile:

```
mkdir nginx
cd nginx
vim Dockerfile
```

Then we add the following:

```
FROM ubuntu:16.04

MAINTAINER Chris Fidao

RUN apt-get update && apt-get install -y nginx \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && echo "daemon off;" >> /etc/nginx/nginx.conf

CMD ["nginx"]
```

Now we can use the docker build command to build this image:

```
docker build -t shippingdocker/nginx:0.1.0 .
```

The video explains how Docker runs new containers, commits any change to a new image, and spins up a new container based on the previous image for future changes. This means every command/directive we put in the Dockerfile adds another layer onto this container. Each layer adds some space and cruft to the container history and data. This is why we attempt to cram as much as we can into each directive, such as RUN.

Docker also caches work it's done before, so if you're careful, you can make subsequent builds super fast by making things that may change on different builds appear last or as far down in the Dockerfile as possible.

We can run a new container from this resulting image just like before as well:

```
docker run -d -p 80:80 -v $(pwd)/../:/var/www/html shippingdocker/nginx:0.1.0
```

We'll see that works in our browser.

Finally, I cover the docker build command a bit more, mentioning that you can set multiple tags (for example, you may want to the newest version to always also be the default latest tag):

```
docker build -t shippingdocker/nginx:0.1.0 -t shippingdocker/nginx:latest .
```

## Build Command Details

Here's some more detail on the docker build command.
Consider this command:

```
docker build -f ./Dockerfile -t shippingdocker/nginx:latest ./build
```

We're telling Docker a few things here:

* build - Build from a Dockerfile
* `-f` ./Dockerfile - If the Dockerfile is in a specific directory or is not named Dockerfile, we can set that file path.
* `-t` shippingdocker/nginx:latest - We can name and tag the resulting image. We can use the -t flag multiple times
* `./build` - This is the "context" and doesn't need to be the same directory as the Dockerfile. Any file path referenced in a Dockerfile will be relative to the context set.

This is used for COPYing or ADDing any external files into a Docker image
