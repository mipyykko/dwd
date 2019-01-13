### 3.1; 3.3

```
$ docker history backend
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
44c888700e2f        7 minutes ago       /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "npm …   0B                  
4b56a567eae9        7 minutes ago       /bin/sh -c #(nop)  USER app                     0B                  
0c2ed024e303        7 minutes ago       /bin/sh -c apt-get update && apt-get install…   225MB               
$ docker history example
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
e616a3c1de13        3 minutes ago       /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "serv…   0B                  
7bd18b5189c8        3 minutes ago       /bin/sh -c #(nop)  USER app                     0B                  
08919c420f89        3 minutes ago       /bin/sh -c apt-get update && apt-get install…   353MB               
```

##### backend Dockerfile
```
FROM ubuntu:16.04

COPY ./ backend/
WORKDIR /backend
EXPOSE 8000

RUN apt-get update && apt-get install -y curl && \
    curl -sL https://deb.nodesource.com/setup_10.x | bash && \
    apt-get install -y nodejs && \
    npm install && \  
    useradd -m app && \
    chown -R app:app /backend

USER app
CMD npm start
```

##### frontend Dockerfile
```
FROM ubuntu:16.04

COPY ./ example/
WORKDIR /example
EXPOSE 5000

RUN apt-get update && apt-get install -y curl && \
    curl -sL https://deb.nodesource.com/setup_10.x | bash && \ 
    apt-get install -y nodejs && \
    useradd -m app && \
    chown -R app:app /example && \ 
    npm install && npm install -g serve && npm run build

USER app

CMD serve -s -l 5000 dist
```

