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
