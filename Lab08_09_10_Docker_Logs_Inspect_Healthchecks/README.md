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

## Step-by-Step Solution with Screenshots and Learnings

### Part 1: Docker Logs & Debugging

#### Step 1: Pull an Image
The lab asked me to pull the latest nginx image to have a base container for testing logs. So, I wrote the command `docker pull nginx:latest`, and it downloaded the image successfully from Docker Hub. This happened quickly, and I learned that pulling images is the first step to get ready-made software for containers.

![Pull the Image](Outputs/1%20Pull%20the%20Image.png)

#### Step 2: Run the Container in Background
Next, the lab instructed me to run the nginx container in the background with a name. I used `docker run -d --name my_nginx nginx:latest`, and the container started detached, meaning it runs in the background without blocking the terminal. I learned that the `-d` flag is useful for long-running services, and naming containers makes them easier to manage.

![Run Container of image pulled in Background](Outputs/2%20Run%20Container%20of%20image%20pulled%20in%20Background.png)

#### Step 3: View Logs
The lab then asked to view the logs of the running container. I ran `docker logs my_nginx`, and it displayed all the output messages from nginx, like startup info and access logs. This showed me how containers log their activities, and I learned that logs are essential for debugging what the app is doing inside the container.

![View Logs of container](Outputs/3%20View%20Logs%20of%20container.png)

#### Step 4: Follow Logs in Real-Time
To see logs as they happen, the lab had me follow the logs live. I executed `docker logs -f my_nginx`, and it streamed new log entries in real-time. This was useful for monitoring ongoing activity, and I learned that the `-f` flag (follow) is great for watching live events in containers.

![Follow Logs Live](Outputs/4%20Follow%20Logs%20Live.png)

#### Step 5: Get Shell Access to Troubleshoot
For deeper troubleshooting, the lab required shell access inside the container. I used `docker exec -it my_nginx /bin/bash`, which opened an interactive bash shell. Inside, I ran `ps aux` to list processes and `env` to see environment variables. This happened seamlessly, and I learned that `docker exec` allows fixing issues directly in the running container, like checking what's running or modifying files.

![Get Shell Access](Outputs/5%20Get%20Shell%20Access.png)

### Part 2: Docker Inspect & Metadata

#### Step 6: List Running Containers
The lab asked to list all running containers. I wrote `docker ps`, and it showed the my_nginx container with its ID, image, status, and ports. This gave a quick overview, and I learned that `docker ps` is the go-to command for seeing what's currently active in Docker.

![listing containers using docker ps](Outputs/6%20listing%20containers%20using%20docker%20ps.png)

#### Step 7: Inspect a Container
To get detailed metadata, the lab instructed inspecting a container. I ran `docker inspect <container_id>`, and it returned a JSON object with all config, network, and mount details. This was comprehensive, and I learned that inspect reveals everything about a container's setup, helpful for troubleshooting configuration issues.

![Inspect Container](Outputs/7%20Inspect%20Container.png)

#### Step 8: List Images
Next, the lab had me list all images. I used `docker images`, which displayed the nginx image with size and creation date. This showed available images, and I learned that images are the blueprints for containers, and listing them helps manage disk space.

![List Images using docker images](Outputs/8%20List%20Images%20using%20docker%20images.png)

#### Step 9: Inspect an Image
For image details, the lab asked to inspect an image. I executed `docker inspect <image_id>`, getting JSON with layers, env vars, and more. This was similar to container inspect but for the image itself, and I learned that image inspect helps understand what's built into the image before running.

![Inspect Image](Outputs/9%20Inspect%20Image.png)

### Part 3: Healthchecks in Docker

#### Step 10: Create Project Folder
The lab required creating a new project for healthchecks. I made `mkdir docker-healthcheck-lab && cd docker-healthcheck-lab`, creating the folder and navigating into it. This set up the workspace, and I learned that organizing projects in folders keeps things tidy for Docker builds.

![mkdir docker-healthcheck-lab](Outputs/10%20mkdir%20docker-healthcheck-lab.png)

#### Step 11: Create server.js
To build a simple app, the lab asked for a Node.js server. I created server.js with HTTP server code listening on port 80. This was a basic "Hello World" app, and I learned that healthchecks need an endpoint to monitor, so the server provides that.

![Creating server.js](Outputs/11%20Creating%20server.js.png)

#### Step 12: Create package.json
For dependencies, the lab instructed creating package.json. I wrote the JSON with name, scripts, and deps. This defined the project, and I learned that package.json tells npm what to install and how to start the app.

![Creating package.json](Outputs/12%20Creating%20package.json.png)

#### Step 13: Install Dependencies
The lab had me install npm packages. I ran `npm install`, which downloaded node_modules. This took a moment, and I learned that installing deps is crucial before building, as the image needs them to run the app.

![installing nodejs](Outputs/13%20installing%20nodejs.png)

#### Step 14: Create Dockerfile
To containerize the app, the lab asked for a Dockerfile. I created it with FROM, WORKDIR, COPY, RUN, EXPOSE, CMD, and HEALTHCHECK. This defined the image build, and I learned that HEALTHCHECK automatically monitors the app's health using curl.

![Create Dockerfile (Use Notepad)](Outputs/14%20Create%20Dockerfile%20(Use%20Notepad).png)

#### Step 15: Build Image
The lab required building the image. I used `docker build -t healthcheck-demo .`, and it built successfully with layers. This took time for npm and apt, and I learned that building images packages the app with all dependencies for portability.

![Build Image](Outputs/15%20Build%20Image.png)

#### Step 16: Run Container
To start the app, the lab instructed running the container. I executed `docker run -d --name healthcheck-container healthcheck-demo`, and it started in background. This launched the server, and I learned that detached mode lets containers run independently.

![Run Container using docker run -d --name healthcheck-container healthcheck-demo](Outputs/16%20Run%20Container%20using%20docker%20run%20-d%20--name%20healthcheck-container%20healthcheck-demo.png)

#### Step 17: Check Health Status
Finally, the lab asked to check the health. I ran `docker ps --filter "name=healthcheck-container"`, showing the container as healthy. This confirmed the healthcheck worked, and I learned that health status helps ensure apps are running properly without manual checks.

![check health using docker ps --filter name healthcheck-container](Outputs/17%20check%20health%20using%20docker%20ps%20--filter%20name%20healthcheck-container.png)

#### Step 18: Screenshot of Container Health Showing Healthy Highlighted
As an extra, I captured a screenshot highlighting the healthy status. This reinforced the healthcheck success, and I learned that visual confirmation is useful for documentation.

![screenshot of container health showing healthy highlighted](Outputs/18%20screenshot%20of%20container%20health%20showing%20healthy%20highlighted.png)

#### Step 19: Cleanup Last Image
To clean up, the lab had me remove the image. I used commands to stop and remove containers/images. This freed space, and I learned that cleanup prevents disk clutter in Docker environments.

![Cleanup last image](Outputs/19%20Cleanup%20last%20image%20.png)

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

## Conclusion
In these three labs, I learned how to effectively monitor and manage Docker containers. From pulling images and running containers to viewing logs for debugging and inspecting metadata for deep insights, each part built my understanding of Docker's tools. The healthchecks were particularly valuable, teaching me how to automate monitoring to ensure apps stay healthy and self-healing. Overall, I gained practical skills in troubleshooting, configuration, and maintenance, making me more confident in deploying reliable containerized applications. These labs showed that Docker isn't just about running apps—it's about observing, inspecting, and ensuring they run smoothly in production. I also learned the importance of documentation and screenshots for sharing knowledge, as well as cleaning up resources to keep environments efficient.
