# Loopback 3: Simple Team Management

This is a simple Team Management API built using IBM LoopBack 3. The API allows you to manage teams and their members, with a one-to-many relationship between teams and members. The data is stored in an in-memory database.


## Database
This project uses an in-memory database, which means that the data will be reset every time the server restarts.

## Models

### Team
- **name** (string, required): The name of the team.
- **description** (string): A brief description of the team.

### Member
- **name** (string, required): The name of the member.
- **role** (string, required): The role of the member within the team.

## Relationships
- **Team** has many **Members**.
- **Member** belongs to one **Team**.

---

## Getting Started

## Docker Setup

The application is configured to run in a Docker container, making it easy to set up and manage dependencies.



### Dockerfile

The `Dockerfile` sets up the Node.js environment, installs dependencies, and starts the LoopBack application.

```Dockerfile
FROM danlynn/ember-cli:3.28.0-node_16.8

ARG USERNAME=node

RUN mkdir -p /home/$USERNAME/app
USER $USERNAME    
WORKDIR /home/$USERNAME/app
```

---

### docker-compose.yml

The `docker-compose.yml` file defines the services required to run the application. In this case, it only includes the LoopBack API service.

```yaml
services:
  web:
    build:
      context: .
      dockerfile: Dockerfile
    image: botdistrikt_web
    container_name: botdistrikt_web
    tty: true
    ports:
      - "4200:4200"
    command: sleep infinity
    volumes:
      - /home/alesha/botdistrikt:/home/node/app
    networks:
      - botdistrikt_dev
    restart: unless-stopped

  api:
    build:
      context: .
      dockerfile: api.Dockerfile
    image: botdistrikt_api
    container_name: botdistrikt_api
    volumes:
      - /home/alesha/botdistrikt/bdapi:/home/node/app
    tty: true
    ports:
      - "3000:3000"
    command: sleep infinity
    networks:
      - botdistrikt_dev
    restart: unless-stopped
networks:
  botdistrikt_dev:
    external: true
```

### Installation
1. Clone the repository:
   ```bash
   git clone THIS_REPO
   ```
2. Edit volume on the api service in docker compose according to your folder structure
   
  ```bash
  docker-compose up -d --build
  docker-compose exec api bash
  npm install
  node .
  ```
 - **`docker-compose up -d --build`**: Builds and starts the Docker containers in detached mode (running in the background).
 - **`docker-compose exec api bash`**: Opens an interactive bash shell inside the running `api` container.
 - **`npm install`**: Installs all Node.js dependencies listed in `package.json` inside the container.
 - **`node .`**: Starts the LoopBack application (assuming the entry point is defined in the current directory).
   
The API will be running at `http://localhost:3000`.

### API Endpoints

#### Teams
- **GET /teams**: List all teams.
- **POST /teams**: Create a new team.
- **GET /teams/{id}**: Get details of a specific team.
- **PATCH /teams/{id}**: Update a specific team.
- **DELETE /teams/{id}**: Delete a specific team.

#### Members
- **GET /members**: List all members.
- **POST /members**: Create a new member.
- **GET /members/{id}**: Get details of a specific member.
- **PATCH /members/{id}**: Update a specific member.
- **DELETE /members/{id}**: Delete a specific member.

### Example Requests

#### Create a Team
```bash
curl -X POST -H "Content-Type: application/json" -d '{
  "name": "Hello Team",
  "description": "Have fun guys."
}' http://localhost:3000/teams
```

#### Create a Member
```bash
curl -X POST -H "Content-Type: application/json" -d '{
  "name": "John",
  "role": "Developer",
  "teamId": 1
}' http://localhost:3000/members
```

#### Get All Teams
```bash
curl -X GET http://localhost:3000/teams
```

#### Get All Members
```bash
curl -X GET http://localhost:3000/members
```
for exploring API please visit 
```
http://localhost:3000/explorer
```

![image](https://github.com/user-attachments/assets/a6f33d37-82b6-4b64-a52c-3319ad8a970d)



---
