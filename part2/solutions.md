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
