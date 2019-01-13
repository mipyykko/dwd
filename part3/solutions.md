### 3.1; 3.3

##### before
```
backend                  latest              db206f291bf0        12 seconds ago      343MB
example                  latest              3f0ea36f32ea        2 seconds ago       470MB
```

##### after
```
backend                  latest              5ddc5bc2a065        8 seconds ago       317MB
example                  latest              eca2c3137879        40 seconds ago      445MB
```

##### backend Dockerfile
```
FROM ubuntu:16.04

COPY ./ backend/
WORKDIR /backend
EXPOSE 8000

RUN apt-get update && apt-get install -y curl && \
    curl -sL https://deb.nodesource.com/setup_10.x | bash && \
    apt-get purge -y --auto-remove curl && \
    apt-get install -y nodejs && \
    npm install && \  
    useradd -m app && \
    chown -R app:app /backend && \
    rm -rf /var/lib/apt/lists/*
    
USER app
CMD npm start
```

##### frontend Dockerfile
```
FROM ubuntu:16.04

COPY ./ example/
WORKDIR /example
EXPOSE 5000

RUN apt-get update && apt-get install -y curl ca-certificates && \
    curl -sL https://deb.nodesource.com/setup_10.x | bash && \ 
    apt-get purge -y --auto-remove curl && \
    apt-get install -y nodejs && \
    useradd -m app && \
    chown -R app:app /example && \ 
    npm install && npm install -g serve && npm run build && \
    rm -rf /var/lib/apt/lists/*

USER app

CMD serve -s -l 5000 dist
```

### 3.4

##### before:
see 3.1

##### after:
```
backend                  latest              d6c623c1612a        3 seconds ago        124MB
example                  latest              365053bace15        2 seconds ago       257MB
```

##### backend Dockerfile:
```
FROM node:alpine

COPY ./ backend/
WORKDIR /backend
EXPOSE 8000
ENV NODE_ENV production
ENV NPM_CONFIG_PREFIX=/home/node/.npm-global
ENV PATH=$PATH:/home/node/.npm-global/bin

RUN npm install -g cross-env && \
    npm install && \
    chown -R node:node /backend

USER node
CMD npm start
```

##### frontend Dockerfile
```
FROM node:alpine

COPY ./ example/
WORKDIR /example
EXPOSE 5000
ENV NODE_ENV production

RUN chown -R node:node /example && \
    npm install && npm install -g serve && npm run build

CMD serve -s -l 5000 dist
```
