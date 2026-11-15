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
