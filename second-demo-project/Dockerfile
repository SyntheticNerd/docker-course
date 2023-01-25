FROM node

#this tells docker that all comands should be executed in the app directory
WORKDIR /app

# First we copy the package.json taking advantage of Dockers cached layer system
COPY package.json /app
# Then we run npm install which will be cached and only ran if the package.json has changed
RUN npm install

# using copy . . specifies two paths first . is everything inside the root the second is a folder in the image container which we called app
#a ./ specifies the rewlative path to the current WORKDIR(woring directory) in this container which is set to /app
#we are going to stick to the absolute path rather than the relative path
COPY . /app

# this like a commenmt on wich port will be exposed to our local machine that the application is running on
EXPOSE 80

# If you dont sepcify a CMD the base image will run; no base image and no CMD will result in an error
# CMD is an array of strings using ""(double quotes)
CMD ["node", "server.js"]

#! TERMINAL COMMANDS
#? 1. docker build . //tells docker to build a new custom image based on our Dockerfile
#? 2. copy the i.d. that was generated for you  and in the terminal do:
#?        - docker run a7189a9eb23a14769d68722757ab14c6a1a8c6d7507ffa3c4213bc0113a4c655 (which is your i.d. number; might look different for windows vs mac users)
#?        - BUT THIS WONT EXPOSE THE PORT in order to properly expose the port you need to use the -p (publish) tag and specify what port you want it accessed
#?        - locally and which port the application was given access to something like 3000:80
#?        - bring that all together and you get: docker run -p 3000:80 a7189a9eb23a14769d68722757ab14c6a1a8c6d7507ffa3c4213bc0113a4c655
#?        - you can use docker run -d i.d. to launch in detached mode
#? 3. To stop the container in a new terminal run: docker stop container_name
#?        - you can find the running container name either in docker desktop or by tytping: docker ps
#? 4. You can restart a container with: docker start container_name
#?        - This is different than docker run in that it is in detached mode
#?        - Use docker start -a <id>|<name> to start in attached mode
#?        - Detach mode means that the container runs in the background rather than in your terminal
#? 5. You can attach a container running in the background with:
#?        - docker attach <id>|<name>
#? 6. You can see the logs of a detached container:
#?        - docker logs <id>|<name>
#?        - or use: docker attach -f <id>|<name>
#?            - This will print out the logs and leave you in attchaed mode to see future logs

#* Side note you dont have to use the full docker i.d. just enough character to uniqely identify it.

# Currently anytime we want to change our code we have to rebuild our image because copy takes a snapshot of our code and copies it when the image is built : "docker build ."
# Docker uses a layer based cacheing system each line in our Dockerfile represents a different layer and when one line has had changes and need to reexecute because its not cached all subsequent
# lines need to be re-executed, this gives us a chance to optomize the node_modules and package.json file so we copt the packeage.json first run npm install then copy our app files
# this ensures that npm install only runs if package.json has changed

#! Dont forget about using docker --help to see available commands