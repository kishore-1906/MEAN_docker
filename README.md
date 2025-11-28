# MEAN CRUD App – Docker + CI/CD + AWS Deployment

This is a simple MEAN (MongoDB, Express, Angular, Node.js) CRUD application that is fully containerized using Docker, deployed on AWS EC2, and automated using GitHub Actions CI/CD.

---

## Features

- Angular frontend (CRUD UI)
- Node.js + Express backend API
- MongoDB database
- Docker images for frontend and backend
- Docker Compose for multi-container deployment
- CI/CD pipeline using GitHub Actions
- Automatic deployment to EC2
- Nginx serving Angular UI on port 80

---

## Project Structure

backend/   → Express API
frontend/  → Angular UI
docker-compose.yml
.github/workflows/cicd.yml

---

## Docker

### Backend Dockerfile

FROM node:18-alpine WORKDIR /usr/src/app COPY package*.json ./ RUN npm install --only=production COPY . . EXPOSE 3000 CMD ["node", "server.js"]

### Frontend Dockerfile

FROM node:18-alpine AS build WORKDIR /app COPY package*.json ./ RUN npm install COPY . . RUN npm run build -- --configuration=production

FROM nginx:alpine COPY --from=build /app/dist/angular-15-crud /usr/share/nginx/html EXPOSE 80

---

## Docker Compose

version: "3.9"

services: mongo: image: mongo:6 environment: MONGO_INITDB_ROOT_USERNAME: admin MONGO_INITDB_ROOT_PASSWORD: adminpassword

backend: image: kishore190/mean-backend:latest environment: MONGO_URI: mongodb://admin:adminpassword@mongo:27017/dd_db?authSource=admin ports: - "3000:3000"

frontend: image: kishore190/mean-frontend:latest ports: - "80:80"

---

## AWS Deployment Steps

1. Launch an EC2 Ubuntu instance  
2. Install Docker & Docker Compose  
3. Clone the repository:

cd /opt git clone <repo-url> cd mean-app

4. Start services:

docker compose up -d

5. Access the application:

- Frontend → http://EC2_PUBLIC_IP  
- Backend API → http://EC2_PUBLIC_IP:3000/api/tutorials  

---

## CI/CD – GitHub Actions

The pipeline:

- Builds Docker images  
- Pushes to Docker Hub  
- SSH into EC2  
- Pulls updated images  
- Restarts containers  

Workflow file: .github/workflows/cicd.yml

---

## Important Note

Do *NOT delete* the EC2 instance.  
