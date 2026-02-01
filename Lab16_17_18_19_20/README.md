# Lab 16 | 17 | 18 | 19 | 20: Docker Security, ARG vs ENV, Desktop GUI, Private Registry, and CI/CD

## What You Will Learn
- How to make Docker containers safer by running as normal users and limiting powers.
- The difference between ARG (build-time only) and ENV (runtime settings) in Dockerfiles.
- Using Docker Desktop's visual interface to manage containers and images.
- Setting up your own private image storage and pushing/pulling images.
- How Docker fits into automatic CI/CD pipelines for building, testing, and deploying apps.

## Key Concepts (Explained Simply)
- **Docker Security Basics**: Containers should not run as root (admin) to prevent full system access if hacked. Drop extra capabilities (powers) and limit resources like memory/CPU.
- **ARG vs. ENV**: ARG is for values needed only during building (like software version), gone after build. ENV sets variables that persist in the running container (like database URLs).
- **Docker Desktop GUI**: A visual app to see and control containers/images without typing commands. Good for beginners, works with CLI.
- **Private Registry**: Your own local server to store Docker images privately. Tag and push images there, pull when needed.
- **Docker in CI/CD**: Automate workflows: code changes build images, run tests in containers, deploy consistently. Docker Compose helps test multi-part apps.

---

## Step-by-Step Solution with Screenshots and Learnings

### Part 1: Docker Security Basics

#### Step 1: Create Project Folder
The lab asked me to make a new folder for the security project. So, I created `docker-security-basics` and went inside it. This set up a dedicated space for secure container practices, and I learned that organizing projects helps focus on security from the start.

![Create Project Folder](Outputs/1.png)

#### Step 2: Create Dockerfile
Next, the lab wanted a Dockerfile with a non-root user. I made it with Ubuntu, added a user, switched to it, and set the command. This ensures the container runs as appuser, not root, limiting potential damage. I learned that USER in Dockerfile is key for security.

![Create Dockerfile](Outputs/2.png)

#### Step 3: Create Script
The lab asked for a simple script. I created `example-script.sh` that echoes the current user. This script proves the container is running as the non-root user. I learned that scripts can verify security settings.

![Create Script](Outputs/3.png)

#### Step 4: Build Image
The lab instructed building the image. I ran `docker build -t non-root-user-example .`, which created the secure image. This took time for Ubuntu, and I learned that building includes user setup for safety.

![Build Image](Outputs/4.png)

#### Step 5: Run Container
To test, the lab had me run the container. I used `docker run --rm non-root-user-example`, and it output "Running as user: appuser". This confirmed non-root execution, and I learned that --rm cleans up after for security.

![Run Container](Outputs/5.png)

#### Step 6: Drop Capabilities
The lab asked to drop powers. I ran `docker run --rm --cap-drop=NET_ADMIN alpine:latest ip link add dummy0 type dummy`, and it failed. This showed how dropping NET_ADMIN prevents network changes, and I learned that capabilities control what containers can do.

![Drop Capabilities](Outputs/6.png)

#### Step 7: Add Capabilities
For contrast, the lab wanted adding powers. I ran `docker run --rm --cap-add=NET_ADMIN alpine:latest ip link add dummy0 type dummy`, and it succeeded. This demonstrated adding only needed powers, and I learned to balance security with functionality.

![Add Capabilities](Outputs/7.png)

#### Step 8: Limit Resources
The lab asked to limit memory and CPU. I used `docker run --rm --memory="256m" --cpus="1" alpine:latest sleep 10`. It ran with limits, and I learned that resource limits prevent abuse and keep systems stable.

![Limit Resources](Outputs/8.png)

### Part 2: Dockerfile ARG vs. ENV

#### Step 9: Create Project Folder
The lab required a new folder. I made `docker-arg-env-demo` and entered it. This isolated the demo, and I learned that separate folders avoid conflicts.

![Create Project Folder](Outputs/9.png)

#### Step 10: Create Dockerfile
The lab wanted ARG and ENV. I wrote the Dockerfile with ARG for version and ENV for environment. The RUN echoed the ARG during build. I learned that ARG is temporary, ENV permanent.

![Create Dockerfile](Outputs/10.png)

#### Step 11: Create app.py
For the app, the lab asked for a Python script. I created it to print the ENV variable. This showed runtime access to ENV, and I learned that ENV persists after build.

![Create app.py](Outputs/11.png)

#### Step 12: Build with ARG
The lab instructed building with a custom ARG. I ran `docker build --build-arg APP_VERSION=2.0.0 -t arg-env-demo .`. It used 2.0.0 in the echo, and I learned that --build-arg overrides defaults.

![Build with ARG](Outputs/12.png)

#### Step 13: Run Container
To see ENV, the lab had me run it. I used `docker run --rm arg-env-demo`, printing "Application environment: production". This confirmed ENV works at runtime, and I learned the difference from ARG.

![Run Container](Outputs/13.png)

### Part 3: Docker Desktop GUI (Optional)

#### Step 14: Install Docker Desktop
The lab asked to install Docker Desktop. I downloaded and installed it, then ran `docker --version` to verify. It showed the version, and I learned that GUI complements CLI for visual management.

![Install Docker Desktop](Outputs/14.png)

#### Step 15: Explore Containers Panel
The lab wanted exploring the GUI. I opened Containers tab and saw running ones. This visual view helped, and I learned that GUI is user-friendly for monitoring.

![Explore Containers Panel](Outputs/15.png)

#### Step 16: Explore Images Panel
Next, the lab asked for Images tab. I viewed and searched images. This showed local storage, and I learned that GUI makes pulling images easy.

![Explore Images Panel](Outputs/16.png)

### Part 4: Pushing Images to a Private Registry (Optional)

#### Step 17: Pull Registry Image
The lab required pulling the registry. I ran `docker pull registry:2`. It downloaded the image, and I learned that registries store images privately.

![Pull Registry Image](Outputs/17.png)

#### Step 18: Run Local Registry
To start the registry, the lab had me run it. I used `docker run -d -p 5000:5000 --name myregistry registry:2`. It started on port 5000, and I learned that local registries are like personal hubs.

![Run Local Registry](Outputs/18.png)

#### Step 19: Tag Image
The lab asked to tag an image. I ran `docker images` to list, then `docker tag <image> localhost:5000/myimage`. This labeled it for the registry, and I learned that tagging directs pushes.

![Tag Image](Outputs/19.png)

#### Step 20: Push Image
To upload, the lab instructed pushing. I ran `docker push localhost:5000/myimage`. It uploaded successfully, and I learned that private registries keep images secure.

#### Step 21: Verify
The lab wanted verification. I used `curl -X GET http://localhost:5000/v2/_catalog`, showing myimage. This confirmed storage, and I learned to check registry contents.

### Part 5: Docker in CI/CD Pipeline

#### Step 22: Create docker-compose.yml
The lab asked for a Compose file. I created it with web and Redis services. This defined multi-container testing, and I learned that Compose orchestrates apps.

#### Step 23: Run Tests
To test, the lab had me run Compose. I used `docker-compose up --abort-on-container-exit --build`. It built and tested, then stopped. I learned that ephemeral tests ensure clean runs.

#### Step 24: Discuss Consistency
The lab asked to think about consistency. Containers keep environments uniform, avoiding "works on my machine" issues. I learned that isolation is key for reliable CI/CD.

---

## What Each Command Does (Easy Words)
- `mkdir folder && cd folder`: Makes folder and enters. Like new room.
- `docker build -t name .`: Builds image from Dockerfile. Like cooking recipe.
- `docker run --rm image`: Runs and deletes after. Like quick test.
- `docker run --cap-drop=CAP image cmd`: Runs without powers. Like removing tools.
- `docker run --cap-add=CAP image cmd`: Adds specific powers. Like giving keys.
- `docker run --memory="256m" image`: Limits resources. Like small limits.
- `docker build --build-arg VER=val .`: Builds with custom ARG. Like choosing option.
- `docker run image`: Runs and shows ENV. Like reading label.
- `docker pull registry:2`: Downloads registry. Like getting box.
- `docker run -d -p 5000:5000 registry:2`: Starts registry server. Like opening store.
- `docker tag image localhost:5000/name`: Labels for registry. Like addressing.
- `docker push localhost:5000/name`: Sends to registry. Like shipping.
- `curl http://localhost:5000/v2/_catalog`: Checks contents. Like inventory.
- `docker-compose up --build`: Runs multi-container test. Like group run.

---

## Conclusion
In these five labs, I learned how to secure Docker containers by using non-root users, managing capabilities, and limiting resources to reduce risks. I understood ARG for build-time variables and ENV for runtime configs, making Dockerfiles flexible. The GUI in Docker Desktop provided an easy visual way to manage images and containers, complementing the CLI. Setting up a private registry taught me to store and share images securely without public hubs. Finally, integrating Docker into CI/CD pipelines with Compose ensured consistent testing and deployment, eliminating environment discrepancies. Overall, these labs built my skills in secure, efficient, and automated container management, crucial for modern DevOps and reliable app delivery. I also learned the value of visual documentation and step-by-step learning for sharing knowledge and troubleshooting.