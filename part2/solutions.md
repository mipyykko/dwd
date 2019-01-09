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
