# Local development of a React application using Docker
**Author:** [Alan Mills]
**Date:** [14 July 2017 15:43]
**Tags:** [React], [Docker]
**Status**: Draft

Following the tenants of Continous Delivery, it is idea to develop applications in an environment as similar to production as possible.  I also like to be able to develop on my laptop disconnected from the cloud!  I also like to keep my laptop clean of development libraries, tools, etc as this reduces the number of times that I have to rebuild my laptop.

There are a number of requirements that I wanted to achieve with this setup:
1. I want the source code to be stored on my local laptop
2. I want to edit the source code in the Docker container using Vim
3. I want the node modules to be stored on the Docker container so they are destroyed with the container
4. I want the NPM packages to be installed when the container runs
5. I want the development server (Webpack Dev Server) to run in the docker container
6. I want to access the Webpack Dev Server using a browser running on my host laptop

## Create the Docker based development environment
### Dockerfile
``` Dockerfile
FROM node:8.2.1
EXPOSE 8080
VOLUME /src
WORKDIR /src

# Install Unix development tools
RUN apt-get update
RUN apt-get install -y \
  vim

# Install global development tools from NPM
RUN npm install -g \
  eslint@4.3.0

# Symbolic link node_modules directory to be stored in the container
RUN mkdir -p /root/node_modules
RUN ln -s /root/node_modules /src/node_modules

# Install npm packages and then bash
CMD ["ln -s /root/node_modules /src/node_modules", "bash"]
```

### Build the Docker Image
``` bash
docker build -t webdev:1.0.0 .
```

## Create the React project
1. Create a working location on the host computer for the project
2. Run an instance of our webdev Docker container
3. Setup our React project
4. Run the Webpack Development Server
5. Shut it all down
6. Start is all up again 

``` bash
docker run -it -v `pwd`:/src -p 8080:8080 webdev:1.0.0
mkdir app dist
npm init
npm install --save react react-dom
npm install --save-dev webpack webpack-dev-server babel-core babel-loader babel-preset-env babel-preset-react
vim package.json
:%s/\^//g
/test
$
i
,
"dev": "webpack-dev-server"
<esc>
:w
:e dist/index.html
:w
:e app/index.jsx
:w
:e webpack.config.js
:w
:e .babelrc
:w
```

Open another bash terminal
``` bash
docker exec -it `docker ps -lq` npm run dev
```
