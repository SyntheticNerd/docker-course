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