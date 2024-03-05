# Codechecker with PostgreSQL Deployment Guide

This guide will help you deploy a Codechecker web application with a PostgreSQL database using Docker and Docker Compose.

## Prerequisites
Before you begin, ensure you have the following installed:

- Docker
- Docker Compose

## Setup Instructions
### 1. Create Docker Compose File
Create a 'docker-compose.yml' file in your project directory with the following content:
```docker
version: '3.8'

services:
  codechecker-web:
    image: codechecker/codechecker-web:latest
    environment:
      DATABASE: "postgresql"
      POSTGRES_DB: codechecker_db
      POSTGRES_USER: codechecker_user
      POSTGRES_PASSWORD: your_password
    ports:
      - "8001:8001"
    depends_on:
      - db

  db:
    image: postgres:latest
    environment:
      POSTGRES_DB: codechecker_db
      POSTGRES_USER: codechecker_user
      POSTGRES_PASSWORD: your_password
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```
Replace 'your_password' with a secure password of your choice.

### 2. Initialize the Database & Start the Codechecker Service
Start your PostgreSQL container to initialize it with the required database and user. This step ensures the database is ready before starting the Codechecker service. Once the database is up and running, start the entire application:
```docker
docker-compose up -d db
```
This command will start both the Codechecker web service and the PostgreSQL database service.

### 3. Access Codechecker Web Interface
After the services have started, open your web browser and navigate to 'http://localhost:8001? to access the Codechecker web interface.

### 4. Shutdown and Cleanup
To stop and remove all the running containers, use the following Docker Compose command:

```docker
docker-compose down
```