# Lab 08 | 09 | 10: Docker Logs & Debugging, Inspect & Metadata, Healthchecks

## What You Will Learn
- How to check logs from Docker containers to see what's happening inside.
- How to look at detailed info about containers and images using inspect.
- How to set up health checks to make sure containers are working properly.
- How to fix problems in containers using commands like exec.

## Key Concepts (Explained Simply)
- **Docker Logs**: Like a diary for your container – shows messages from the app running inside.
- **Docker Inspect**: A way to get all the details about a container or image, like its settings, networks, and files.
- **Healthchecks**: Automatic checks to see if your app is healthy, so Docker can restart it if it's not.
- **Docker Exec**: Lets you run commands inside a running container to fix things.

---

## Step-by-Step Solution

### Part 1: Docker Logs & Debugging

#### 1. Pull an Image
```
docker pull nginx:latest
```
(This downloads the latest nginx image from Docker Hub.)

#### 2. Run the Container in Background
```
docker run -d --name my_nginx nginx:latest
```
(This starts nginx in the background with a name "my_nginx". -d means detached/background.)

#### 3. View Logs
```
docker logs my_nginx
```
(This shows all logs from the container.)

#### 4. Follow Logs in Real-Time
```
docker logs -f my_nginx
```
(This shows logs live as they happen. -f means follow.)

#### 5. Get Shell Access to Troubleshoot
```
docker exec -it my_nginx /bin/bash
```
(This opens a shell inside the container. -it means interactive terminal.)

Inside the shell:
```
ps aux
```
(This lists running processes in the container.)

```
env
```
(This shows environment variables.)

### Part 2: Docker Inspect & Metadata

#### 1. List Running Containers
```
docker ps
```
(This shows all running containers.)

#### 2. Inspect a Container
```
docker inspect <container_id>
```
(This gives detailed JSON info about the container, like config, networks, mounts.)

#### 3. List Images
```
docker images
```
(This shows all images.)

#### 4. Inspect an Image
```
docker inspect <image_id>
```
(This gives details about the image.)

#### 5. Parse Metadata (Optional)
```
docker inspect <container_id> | jq '.[0].Config.Env'
```
(This uses jq to show environment variables nicely.)

### Part 3: Healthchecks in Docker

#### 1. Create Project Folder
```
mkdir docker-healthcheck-lab && cd docker-healthcheck-lab
```
(This makes a folder and goes into it.)

#### 2. Create server.js
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

server.listen(80, () => {
  console.log('Server running at http://localhost:80/');
});
```

#### 3. Create package.json
```json
{
  "name": "docker-healthcheck-lab",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "http": "^0.0.1-security"
  }
}
```

#### 4. Install Dependencies
```
npm install
```
(This installs the needed packages.)

#### 5. Create Dockerfile
```dockerfile
FROM node:14
WORKDIR /usr/src/app
COPY . .
RUN npm install
EXPOSE 80
CMD ["npm", "start"]
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost:80/ || exit 1
```

#### 6. Build Image
```
docker build -t healthcheck-demo .
```
(This builds the image with healthcheck.)

#### 7. Run Container
```
docker run -d --name healthcheck-container healthcheck-demo
```
(This starts the container with health monitoring.)

#### 8. Check Health Status
```
docker ps --filter "name=healthcheck-container"
```
(This shows if the container is healthy.)

---

## What Each Command Does (Easy Words)
- `docker pull nginx:latest`: Downloads the nginx image.
- `docker run -d --name my_nginx nginx:latest`: Starts nginx in background.
- `docker logs my_nginx`: Shows container's message history.
- `docker logs -f my_nginx`: Shows live logs.
- `docker exec -it my_nginx /bin/bash`: Opens a terminal inside the container.
- `ps aux`: Lists processes running in the container.
- `env`: Shows settings inside the container.
- `docker ps`: Lists running containers.
- `docker inspect <id>`: Shows detailed info about container/image.
- `docker images`: Lists downloaded images.
- `mkdir docker-healthcheck-lab && cd docker-healthcheck-lab`: Makes folder and enters it.
- `npm install`: Installs app dependencies.
- `docker build -t healthcheck-demo .`: Builds image from Dockerfile.
- `docker run -d --name healthcheck-container healthcheck-demo`: Starts container with healthcheck.
- `docker ps --filter "name=healthcheck-container"`: Shows container health status.

---

## Outputs & Screenshots
(Add screenshots here for each step, like running commands, logs output, inspect JSON, health status.)

---

## What I Learned
- How to use logs to debug containers.
- How inspect gives deep info for troubleshooting.
- How healthchecks keep apps running smoothly.
- Combining these makes Docker management easier.

---

## Conclusion
You learned to debug, inspect, and monitor Docker containers. These skills help keep apps healthy and fix problems fast!
