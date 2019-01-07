### 1.1
```
$ docker ps -a
CONTAINER ID        IMAGE                         COMMAND                  CREATED              STATUS                      PORTS               NAMES
d3a723b23a69        nginx                         "nginx -g 'daemon of…"   37 seconds ago       Up 36 seconds               80/tcp              competent_kalam
1a6b1f4e220b        nginx                         "nginx -g 'daemon of…"   39 seconds ago       Exited (0) 15 seconds ago                       pensive_williamson
8dd466137688        nginx                         "nginx -g 'daemon of…"   About a minute ago   Exited (0) 6 seconds ago                        optimistic_bardeen
```

### 1.2
```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```

### 1.3
```
$ docker run -d -it --name curler ubuntu:16.04 sh -c 'read website; sleep 3; curl http://$website;'
5487da814561cfdfcbe316247717917aa9ef5032afef88fdc2e2e404158d6684
$ docker exec -it curler bash
root@5487da814561:/# apt-get update
...
root@5487da814561:/# apt-get install curl
...
root@5487da814561:/# exit
exit
$ docker attach curler
helsinki.fi
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="http://www.helsinki.fi/">here</a>.</p>
</body></html>
```

### 1.4

#### Dockerfile
```
FROM ubuntu:16.04

WORKDIR /
RUN apt-get update && apt-get install -y curl
COPY curler.sh .
RUN chmod u+x curler.sh
CMD ["./curler.sh"]
```

```
$ docker run -it curler
helsinki.fi
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="http://www.helsinki.fi/">here</a>.</p>
</body></html>
```

### 1.5

#### Dockerfile
```
FROM ubuntu:16.04

COPY ./ example/
WORKDIR /example
EXPOSE 5000

RUN apt-get update && apt-get install -y curl
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt-get install -y nodejs

RUN npm install && npm install -g serve && npm run build

CMD serve -s -l 5000 dist
```

### 1.6

#### Dockerfile
```
FROM ubuntu:16.04

COPY ./ backend/
WORKDIR /backend
EXPOSE 8000

RUN apt-get update && apt-get install -y curl 
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt-get install -y nodejs

RUN npm install

CMD npm start
```

```
$ docker run -v ${pwd}:/backend -p 8000:8000 backend
```

