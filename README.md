## Final project of Docker course.

## Run application

1. Run prod version:
   ```bash
   docker-compose --profile prod up -d
   ```

2. Run dev version:
   ```bash
   docker-compose --profile dev up -d
   ```

3. Endpoints:
   - Frontend: http://localhost
   - API Backend: http://localhost:8081

## Build images

```bash
docker build -t backend:latest ./backend

docker build -t frontend:latest ./frontend
```