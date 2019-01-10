### 2.1

##### docker-compose.yml
```
version: '3.5'

services:
  backend:
    image: backend
    environment:
      - FRONT_URL=http://localhost:5000
    ports:
      - 8000:8000
  frontend:
    image: example
    environment:
      - API_URL=http://localhost:8000
    ports:
     - 5000:5000
```

### 2.2

```
services:
  redis:
    image: redis
    container_name: redis
    ports:
     - "6379"
  backend:
    image: backend
    environment:
      - FRONT_URL=http://localhost:5000
      - REDIS=redis
    ports:
      - 8000:8000
    links:
      - redis
  frontend:
    image: example
    environment:
      - API_URL=http://localhost:8000
    ports:
      - 5000:5000
```

### 2.3

```
version: '3.5'

services:
  redis:
    image: redis
    container_name: redis
    ports:
     - "6379"
  db:
    image: postgres
    container_name: db
    restart: always
    environment:
      - POSTGRES_PASSWORD=password
  backend:
    image: backend
    environment:
      - FRONT_URL=http://localhost:5000
      - REDIS=redis
      - DB_USERNAME=postgres
      - DB_PASSWORD=password
      - DB_NAME=postgres
      - DB_HOST=db
    ports:
      - 8000:8000
    links:
      - redis
  frontend:
    image: example
    environment:
      - API_URL=http://localhost:8000
    ports:
      - 5000:5000
```

### 2.4
```
version: "3.5"

services:
  ml-frontend:
    image: ml-kurkkumopo-frontend
    container_name: ml_frontend
    links:
      - ml_backend
    ports:
      - 3000:3000
  ml_backend:
    image: ml-kurkkumopo-backend
    container_name: ml_backend
    links:
      - ml_training
    volumes:
      - "./ml-kurkkumopo-training/model:/src/model"
    ports: 
      - 5000:5000
  ml_training:
    image: ml-kurkkumopo-training
    container_name: ml_training
    volumes:
      - "./ml-kurkkumopo-training:/src"
```

### 2.5
```
version: '3.5'

services:
  redis:
    image: redis
    container_name: redis
    ports:
     - "6379"
  db:
    image: postgres
    container_name: db
    restart: always
    environment:
      - POSTGRES_PASSWORD=password
  backend:
    image: backend
    container_name: backend
    environment:
      - REDIS=redis
      - DB_USERNAME=postgres
      - DB_PASSWORD=password
      - DB_NAME=postgres
      - DB_HOST=db
    ports:
      - 8000:8000
    links:
      - redis
  frontend:
    image: example
    container_name: frontend
    ports:
      - 5000:5000
  proxy:
    image: nginx
    container_name: proxy
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro  
    ports:
      - 80:80
```
