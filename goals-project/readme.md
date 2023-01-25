# Docker Stuff

## TERMINAL COMMANDS

1. docker build . //tells docker to build a new custom image based on our Dockerfile
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

## Side notes

You don't have to use the full docker i.d. just enough character to uniquely identify it

Currently anytime we want to change our code we have to rebuild our image because copy takes a snapshot of our code and copies it when the image is built : "docker build ."

Docker uses a layer based caching system each line in our Dockerfile represents a different layer and when one line has had changes and need to re-execute because its not cached all subsequent lines need to be re-executed.

This gives us a chance to optimize the node_modules and package.json file so we copy the package.json first run npm install then copy our app files this ensures that npm install only runs if package.json has changed

Don't forget about using docker --help to see available commands
