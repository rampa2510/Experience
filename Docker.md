# Tips when using with nginx and
## React
1) When using docker with react we can make 2 dockerfiles one for production and one for development.



- Development :-


Directory structure - 
```
.
|───client
|    │   Dockerfile.dev
|    │   package.json
|    |───src   
|____|───public   
|      
├───nginx
│    |default.conf
|    |
│____|Dockerfile.dev
|
|    docker-compose-dev.yml
```

In the development version we will start the development server in react

Dockerfile.dev (inside the client dir) :-

docker-compose-dev.yml :-
```yml
version: "3"

services:
  nginx:
    build: 
      context: ./nginx
      dockerfile: Dockerfile.dev

    restart: always
    depends_on:
      - client
    ports:
      - "3050:80"

  client:
    tty: true
    build: 
      context: ./client
      dockerfile: Dockerfile.dev
    restart: always
    volumes: 
      - /app/node_modules
      - ./client:/app
``` 

Dockerfile.dev (in client dir):-
```docker
FROM node:alpine

WORKDIR /app

COPY ./package.json ./

RUN npm i 

COPY ./ ./

CMD ["npm","start"]
```

Now in the ngnix directory the default.conf will look something like this

default.conf :-
```nginx
upstream react {
    server covidian_client:3000;
}

upstream api {
    server covidian_server:4000;
}

upstream predict {
    server covidian_predict:80;
}

server{
    listen 80;

    location / {
        proxy_pass http://react;
    }


    location /sockjs-node {
        proxy_pass http://react;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
    }
}
```

Dockerfile.dev:- 
```docker
FROM nginx

COPY ./default.conf /etc/nginx/conf.d/default.conf
```

- Production :- 

Directory structure - 
```
.
|───client
|    │   Dockerfile
|    │   package.json
|    |───src   
|    |───public   
|    |───nginx 
|____|___|---default.conf  
|      
├───nginx
│    |default.conf
│____|Dockerfile
|
|    docker-compose.yml
```

docker-compose.yml :-

```yml
version: "3"

services:
  nginx:
    build: ./nginx
    restart: always
    depends_on:
      - client
    ports:
      - ${nginx_external_port}:${nginx_internal_port}

  client:
    tty: true
    build: ./client
    restart: always

```

Dockerfile (in client dir) :-
```docker 
FROM node:alpine as builder
WORKDIR /app
COPY ./package.json ./
RUN npm i
COPY ./ ./
RUN npm run build

FROM nginx

COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf

COPY --from=builder /app/build /usr/share/nginx/html
```
In the root directory of the react project (In this case Client folder) we will have to create a nginx folder which will store the configuration for routing if we are using library like react router dom. In the nginx folder the code will be in default.conf

default.conf :-
```nginx
server {
    listen 3000;

    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
        try_files $uri $uri/ /index.html;
    }
}
```

Now in the nginx directory located in the root directory of project (outside Client) :-
```nginx
upstream react {
    server client:3000;
}


server{
    listen 80;

    location / {
        proxy_pass http://react;
    }
}
```

The deockerfile inside this 
Dockerfile (inside the nginx dir) :-
```docker
FROM nginx

COPY ./default.conf /etc/nginx/conf.d/default.conf
```
