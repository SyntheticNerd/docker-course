# Docker Stuff

## Building a Dockerfile Basics

``` txt
# Starting from an official image
# Find them at https://hub.docker.com/search?image_filter=official&q=
FROM node

# Sets the working directory, i.e. where the commands happen for the project
WORKDIR /app

# First we copy the package.json to the working directory of the image taking advantage of Dockers cached layer system
COPY package.json /app
# Then we run npm install which will be cached and only ran if the package.json has changed
RUN npm install

# using copy . . specifies two paths first . is everything inside the root the second is a folder in the image container which we called app
# a ./ specifies the relative path to the current WORKDIR(working directory) in this container which is set to /app
# we are going to stick to the absolute path rather than the relative path so the root directory followed by the working directory this copies the rest of the files from local to the image
COPY . /app

# this is like a comment on which port will be exposed to our local machine that the application is running on this wont actually expose the port that needs to be done when executing the Docker run command
EXPOSE 80

# If you don't specify a CMD the base image will run; no base image and no CMD will result in an error
# CMD is an array of strings using ""(double quotes)
# should be used as a way of defining default arguments for an ENTRYPOINT command or for executing an ad-hoc command in a container.
# In this case we use it to spin up a node server
CMD ["node", "server.js"]
```

## TERMINAL COMMANDS

1. `docker build .` tells docker to build a new custom image based on our Dockerfile
2. copy the i.d. that was generated for you and in the terminal do
   - `docker run <id>|<name>` (which is your i.d. number; might look different for windows vs mac users)
   - BUT THIS WONT EXPOSE THE PORT in order to properly expose the port you need to use the -p (publish) tag and specify what port you want it accessed locally and which port the application was given access to something like 3000:80
   - bring that all together and you get: `docker run -p 3000:80 <id>|<name>`
   - you can use docker run -d i.d. to launch in detached mode
3. To stop the container in a new terminal run: `docker stop container_name`
   - you can find the running container name either in docker desktop or by typing: `docker ps`
4. You can restart a container with: `docker start <name>|<id>`
   - This is different than docker run in that it is in detached mode
   - Use `docker start -a <id>|<name>` to start in attached mode
   - Detach mode means that the container runs in the background rather than in your terminal
5. You can attach a container running in the background with:
   `docker attach <id>|<name>`
6. You can see the logs of a detached container: `docker logs <id>|<name>`
   - or use: `docker attach -f <id>|<name>`
     - This will print out the logs and leave you in attached mode to see future logs
7. To remove docker images: docker rm \<id>|\<name>
   - NOTE: you can not remove running containers
8. `docker images` will show all the images you have saved locally
   - To Remove images use `docker rmi <imageid>`
   - You can only remove images is the container doesn't exist anymore
9. add the `--rm` tag to a docker run command to ensure a container get removed when it is stopped
    - Example: `docker run -p 3000:80 -d --rm <imageid>`
10. `docker image inspect <imageid>` is a way to inspect the actual image, info like
    - when it was create
    - the image id
    - what OS it uses
11. `docker cp local_folder/. <container_name>:</folderName>` to copy local into container
    - If the folder does not exist it will be created
    - ==**You can use this to copy files into the container but it is BUG PRONE**==
    - There will be a better way later
12. `docker cp <container_name>:</folderName> local_folder` to copy container into local
    - If the folder does not exist it will be created
13. Tagging Images
14. Adding `--name yourName` to a docker run command lets you name your container.
    - `docker run -p 3000:80 -d --rm --name appName <imageid>`
15. When building as image adding a `-t name:tag(optional)` to the docker build command allows you to set a name and a optional tag.
    - `docker build -t name:tag .`
      - don't forget that `.` at the end or specify which file your building an image from
16. 

## Side notes

You don't have to use the full docker i.d. just enough character to uniquely identify it

Currently anytime we want to change our code we have to rebuild our image because copy takes a snapshot of our code and copies it when the image is built : "docker build ."

Docker uses a layer based caching system each line in our Dockerfile represents a different layer and when one line has had changes and need to re-execute because its not cached all subsequent lines need to be re-executed.

This gives us a chance to optimize the node_modules and package.json file so we copy the package.json first run npm install then copy our app files this ensures that npm install only runs if package.json has changed

Don't forget about using docker --help to see available commands
