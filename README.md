# vuejs-notes

## Installation
```
$ npm install -g @vue/cli
$ vue --version
```

### Create a Project
```
$ vue create hello-world
```

Folder Structure
```
~/environment/hello-world
├── node_modules/
├── public/
    ├── favicon.ico
    └── index.html
├── src/
    ├── assets/
        └── logo.png
    ├── components/
        └── HelloWorld.vue
    ├── App.vue
    └── main.js
├── .gitignore
├── babel.config.js
├── package-lock.json
├── package.json
└── README.md
```

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Lints and fixes files
```
npm run lint
```

### Run your tests
```
npm run test
```

### Compiles and minifies for production
```
npm run build
```

### Deploy (Docker and Nginx)
- Create a Dockerfile
```
FROM node:10
COPY ./ /app
WORKDIR /app
RUN npm install && npm run build

FROM nginx
RUN mkdir /app
COPY --from=0 /app/dist /app
COPY nginx.conf /etc/nginx/nginx.conf
```

- Create a .dockerignore file in the root of the project. Setting up the .dockerignore file prevents node_modules and any intermediate build artifacts from being copied to the image which can cause issues during building.
```
**/node_modules
**/dist
```

- Create a nginx.conf file in the root of the project
```
user  nginx;
worker_processes  1;
error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;
events {
  worker_connections  1024;
}
http {
  include       /etc/nginx/mime.types;
  default_type  application/octet-stream;
  log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';
  access_log  /var/log/nginx/access.log  main;
  sendfile        on;
  keepalive_timeout  65;
  server {
    listen       80;
    server_name  localhost;
    location / {
      root   /app;
      index  index.html;
      try_files $uri $uri/ /index.html;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
      root   /usr/share/nginx/html;
    }
  }
}
```

- Build, Run and Test docker image
```
$ docker build . -t my-app
$ docker run -d -p 8080:80 my-app
$ curl localhost:8080
```

### FAQ
- What is Webpack? Javascript module bundler
- What is Babel? Babel is a toolchain that is mainly used to convert ECMAScript 2015+ code into a backwards compatible version of JavaScript in current and older browsers or environments
- What is ESLint? Javascript linting utility
- What is TypeScript? Superset of JavaScript which primarily provides optional static typing, classes and interfaces. One of the big benefits is to enable IDEs to provide a richer environment for spotting common errors as you type the code.
- What is Jest and Mocha for Unit Testing?
- What is Cypress and Nightwatch for E2E Testing?
- What is vue-router?
- What is vuex?
