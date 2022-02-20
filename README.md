# Getting Started with Create React App

Today, We will dockerize a React application. We will set up Docker with auto-reloading for development setup and optimized multistage docker build for production deployment. We can even dockerize Next.js or Gatsby Static builds with the same process.
There are many advantages of using Docker. Right now Docker is the defacto standard of containerizing applications. It is easy to build, package, share, and ship applications with Docker. As Docker images are portable it is easy to deploy the application to any modern cloud provider.
Initialize React application
Let’s start by creating a React application. You can use your existing React project. For this blog post, I am creating a new React application with create-react-app.
$ npx create-react-app react-docker
Here I created a new React app named react-docker. Let's verify the app by running the npm start command inside the project directory.
$ npm start
It will start the app and we can verify by visiting http://localhost:3000 in the browser. The application should be running.


Write Dockerfile.
Now let’s create a Docker image for the React application. We need a Dockerfile to create Docker images. Let’s create a file named Dockerfile in the root directory of the React application.
Dockerfile
FROM node:14-alpine

WORKDIR /app

COPY package.json ./

COPY yarn.lock ./

RUN yarn install --frozen-lockfile

COPY . .

EXPOSE 3000

CMD ["npm", "start"]


Here We are using node v14 alpine as the base image to build and run the application. We are running the npm start command default command which will run the React development server. We also need the .dockerignore file which will prevent node_modules other unwanted files to get copied in the Docker image.
.dockerignore
node_modules
npm-debug.log
Dockerfile
.dockerignore

Let’s build the Docker image from the Dockerfile by running the docker build command. Here we are tagging it with the name react-docker.
$ docker build -t react-docker .
After building the docker images we can verify the image by running the docker images command. We can see an image with the name react-docker is created.
$ docker images
We can run the Docker image with the docker run command. Here we are mapping the system port 3000 to Docker container port 3000. We can verify if the application is running or not by visiting http://localhost:3000.
$ docker run -p 3000:3000 react-docker
Add Docker Compose
The React application is working fine inside the docker container, but we need to build and run the docker container every time we make any changes in the source files as auto-reloading is not working with this setup. We need to mount the local src folder to the docker container src folder, so every time we make any change inside the src folder, it will auto-reload the page on any code changes.
We will add the docker-compose.yml file to the root of the project to mount the local src folder to the /app/src folder of the container.
docker-compose.yml
version: "3"

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - ./src:/app/src
    ports:
      - "3000:3000"
Run the docker-compose up command to start the container. The React development server will be running inside the container and will be watching the src folder.
$ docker-compose up
We can’t ship this docker image to the production as it is not optimized and runs a development server inside. Let’s rename the Dockerfile as Dockerfile.dev and update the docker-compose.yaml file to use the Dockerfile.dev file. We will use docker-compose and the Dockerfile.dev file only for development. We will create a new Dockerfile for the production build.
$ mv Dockerfile Dockerfile.dev
docker-compose.yml


