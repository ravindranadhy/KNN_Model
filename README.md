services:

  web:
    image: nginx:latest
    ports:
      - "8080:80"

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: demo
      POSTGRES_PASSWORD: demo
      POSTGRES_DB: demo_db



services:

  web:
    image: nginx:latest
    ports:
      - "8081:80"
    depends_on:
      - redis

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: demo
      POSTGRES_PASSWORD: demo
      POSTGRES_DB: demo_db

  redis:
    image: redis:alpine




Networking and volume

services:
  web:
    image: nginx:latest
    ports:
      - "8081:80"
    depends_on:
      - db
    networks:
      - app-net

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: demo
      POSTGRES_PASSWORD: demo
      POSTGRES_DB: demo_db
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - app-net

  redis:
    image: redis:alpine

networks:
  app-net:

volumes:
  db-data:


commands
# 1. Start all containers
docker compose up -d

# 2. Check running containers
docker compose ps

# 3. List Docker networks
docker network ls

# 4. List Docker volumes
docker volume ls

# 5. Enter PostgreSQL container
docker compose exec db psql -U demo -d demo_db

# 6. Create students table
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50)
);

# 7. Insert records
INSERT INTO students (name) VALUES ('Ravi');
INSERT INTO students (name) VALUES ('Vamsi');

# 8. Display records
SELECT * FROM students;

# 9. Exit PostgreSQL
\q

# 10. Stop and remove containers
docker compose down

# 11. Start containers again
docker compose up -d

# 12. Enter PostgreSQL again
docker compose exec db psql -U demo -d demo_db

# 13. Verify persistent storage
SELECT * FROM students;

# 14. Exit PostgreSQL
\q

# 15. Check containers
docker ps
