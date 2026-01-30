# Lab 11 | 12 | 13 | 14 | 15: Docker Lifecycle Hooks, GitHub Builds, Minimal Images, Tagging, and Cleanup

## What You Will Learn
- How Docker containers start and run with special commands like ENTRYPOINT and CMD.
- How to build and run Docker apps directly from code on GitHub.
- Why small images are better and how to use tiny base images like Alpine.
- How to name and version your Docker images properly.
- How to clean up old Docker stuff to save space.

## Key Concepts (Explained Simply)
- **ENTRYPOINT and CMD**: ENTRYPOINT is the main command that always runs when a container starts. CMD gives default extra words (arguments) to that command. Together, they decide what the container does first.
- **Building from GitHub**: You can put your app code and Dockerfile on GitHub, then clone it and build the Docker image locally. You can even auto-build with GitHub Actions.
- **Minimal Base Images**: Instead of big images like Ubuntu, use tiny ones like Alpine (only 5MB) to make your containers small and fast.
- **Tagging and Versioning**: Give your images names with versions, like myapp:v1.0, so you know which one is which and can update safely.
- **Prune and Cleanup**: Docker can pile up old images and containers. Prune commands delete the unused ones to free up disk space.

---

## Step-by-Step Solution with Screenshots and Learnings

### Part 1: Docker Container Lifecycle Hooks (ENTRYPOINT and CMD)

#### Step 1: Create the Project Folder
The lab asked me to make a new folder for the project. So, I created `docker-lifecycle-hooks` and went inside it. This set up a clean space for my files, and I learned that organizing folders helps keep projects separate.

![Create the Project Folder](Outputs/1%20%20Create%20the%20Project%20Folder.png)

#### Step 2: Create the Dockerfile
Next, the lab wanted a Dockerfile. I made a file called Dockerfile with this content:

```
FROM alpine:latest
ENTRYPOINT ["echo"]
CMD ["Hello, world!"]
```

This uses a tiny Alpine image. ENTRYPOINT sets "echo" as the command that always runs. CMD gives "Hello, world!" as the default message. I learned that ENTRYPOINT is like the boss command, and CMD is its helper.

![Create the Dockerfile](Outputs/2%20Create%20the%20Dockerfile.png)

#### Step 3: Build the Image
The lab asked to build the image. I ran `docker build -t my-echo-app .`. This command takes my Dockerfile and makes a new image named my-echo-app. It downloads Alpine if needed and sets up the echo command. I learned that building creates the image from my instructions, like baking a cake from a recipe.

![build the image](Outputs/3%20build%20the%20image.png)

#### Step 4: Run the Container (Default)
To start the container, the lab instructed running it without extra words. I used `docker run my-echo-app`, and the output was "Hello, world!" because ENTRYPOINT ran echo with CMD's default. This showed the container starting and doing its job. I learned that run starts the container, and it uses ENTRYPOINT + CMD together.

![Run the Container (Default)](Outputs/Step%204%20%20Run%20the%20Container%20(Default).png)

#### Step 5: Create GitHub Repo
The lab required a GitHub repo for the next part. I went to GitHub.com, made a new public repo called `docker-github-lab`, and added a README. This is like a online folder for my code. I learned that GitHub stores projects safely online.

![Create GitHub Repo](Outputs/5%20Create%20GitHub%20Repo.png)

### Part 2: Building from GitHub in Docker

#### Step 6: Clone the Repo
To download the repo, the lab asked to clone it. I ran `git clone https://github.com/saliksalik/docker-github-lab.git` and cd into it. This copied the repo from GitHub to my computer. I learned that cloning downloads the repo to my computer.

![Clone the Repo](Outputs/Step%206%20Clone%20the%20Repo.png)

#### Step 7: Create app.py
For the app, the lab wanted a Python Flask server. I created app.py with the Flask code that says "Hello, Docker!" when visited. This is a simple web app. I learned that app.py defines what the app does.

![creating pp.py](Outputs/7%20creating%20pp.py.png)

#### Step 8: Create Dockerfile
To containerize the app, the lab asked for a Dockerfile. I made it with Python image, copy files, install Flask, expose port 5000, and run the app. This tells Docker how to set up the app environment. I learned that Dockerfile packages the app for running anywhere.

![docker file](Outputs/8%20docker%20file.png)

#### Step 9: Initialize Git and Push to GitHub
The lab instructed saving changes online. I ran `git init`, `git add .`, `git commit -m "Initial commit"`, added remote, and `git push origin main`. This uploaded my files to GitHub. I learned that git tracks changes and shares them.

![Initialize Git and Push to GitHub](Outputs/Step%209%20Initialize%20Git%20and%20Push%20to%20GitHub.png)

#### Step 10: Save Files to Repo
As part of pushing, the files were saved to the repo. The proof shows they are on GitHub. I learned that pushing makes code available online.

![save files to repo](Outputs/step%2010%20save%20files%20to%20repo.png)

#### Step 11: Build the Image
The lab asked to build the image locally. I used `docker build -t my-flask-app .`, which created the image with Python and Flask. This assembled the app into a container. I learned that building turns code into a runnable image.

![Build the Image](Outputs/step%2011%20Build%20the%20Image.png)

#### Step 12: Run the Container
To start the web app, the lab instructed running the container. I ran `docker run -p 5000:5000 my-flask-app`, mapping port 5000. The app started and I could visit http://localhost:5000. I learned that running launches the app in a container.

![runn the container using docker run -p 50005000 my-flask-app](Outputs/step12%20runn%20the%20container%20using%20docker%20run%20-p%2050005000%20my-flask-app.png)

#### Step 13: Starts the Webapp
The webapp started successfully, showing "Hello, Docker!" in the browser. This confirmed the app was running. I learned that containers can host web apps easily.

![starts the webapp](Outputs/step13%20starts%20the%20webapp.png)

### Part 3: Minimal Base Images

#### Step 14: Create Alpine Dockerfile
The lab asked for a tiny image. I created Dockerfile with Alpine and apk for curl/bash. This makes a small image. I learned that Alpine keeps containers lightweight.

![create a file called Dockerfile](Outputs/14%20create%20a%20file%20called%20Dockerfile.png)

#### Step 15: Build Alpine Image
To build the small image, I ran `docker build -t my-alpine-image .`. It built quickly. I learned that small images load faster.

![Build Alpine Image](Outputs/14%20%20Build%20Alpine%20Image.png)

#### Step 16: Create Ubuntu Dockerfile
For comparison, the lab wanted a bigger image. I made Dockerfile.ubuntu with Ubuntu and apt. This creates a larger image. I learned that Ubuntu is familiar but heavy.

![Create a file called Dockerfile.ubuntu](Outputs/15%20Create%20a%20file%20called%20Dockerfile.ubuntu.png)

#### Step 17: Create Docker-Tagging Folder
The lab asked for a new folder for tagging. I made `docker-tagging` and went inside. This organizes the next project. I learned that folders keep labs separate.

![Create a new folder using mkdir docker-tagging](Outputs/16%20Create%20a%20new%20folder%20using%20mkdir%20docker-tagging.png)

### Part 4: Docker Tagging and Versioning

#### Step 18: Build with Tag
The lab asked to build and tag the image. I ran `docker build -t myapp:v1 .`. This labeled the image v1. I learned that tags version images.

#### Step 19: Push to Docker Hub (Optional)
If I had a Docker Hub account, I would login, tag, and push. This shares images online. I learned that pushing makes images available to others.

### Part 5: Docker Prune & Cleanup

#### Step 20: Prune Images
The lab asked to delete unused images. I ran `docker image prune` for dangling ones. This freed space. I learned that prune removes junk.

![docker image prune](Outputs/docker%20image%20prune.png)

#### Step 21: Prune All Images
For all unused, I used `docker image prune -a`. This deleted more. I learned that -a is thorough.

![docker image prune -a](Outputs/docker%20image%20prune%20-a.png)

#### Step 22: Prune Volumes
To clean volumes, I ran `docker volume prune`. This removed unused storage. I learned that volumes need cleanup too.

![docker volume prune](Outputs/docker%20volume%20prune.png)

#### Step 23: System Prune
For big cleanup, I used `docker system prune`. This cleared images, containers, networks. I learned that system prune is a full reset.

![docker system prune](Outputs/docker%20system%20prune.png)

#### Step 24: Proof of Files on Repo
Finally, the files were confirmed on GitHub. This showed the repo was set up. I learned that GitHub hosts code reliably.

![proof of files being saved on repo on github](Outputs/10%20proof%20of%20files%20being%20saved%20on%20repo%20on%20github.png)

---

## What Each Command Does (Easy Words)
- `mkdir folder && cd folder`: Makes a new folder and goes inside. Like creating a room for your stuff.
- `docker build -t name .`: Builds an image from Dockerfile with a name. Like assembling a toy from instructions.
- `docker run image`: Starts a container from the image. Like turning on a game console.
- `git clone url`: Downloads a project from GitHub to your computer. Like borrowing a book from the library.
- `git add . && git commit -m "msg" && git push`: Saves changes and uploads to GitHub. Like saving and sharing your work.
- `docker run -p 5000:5000 image`: Runs and connects port 5000. Like opening a door to the app.
- `apk add --no-cache pkg`: Installs packages in Alpine without cache. Like adding light tools.
- `apt-get update && apt-get install pkg`: Updates and installs in Ubuntu. Like shopping for tools.
- `docker images`: Shows all images with sizes. Like listing all your saved photos.
- `docker build -t name:tag .`: Builds with a version tag. Like labeling a photo.
- `docker login && docker push name`: Logs in and uploads image. Like posting a photo online.
- `docker ps -a`: Shows all containers. Like seeing all your running apps.
- `docker image prune`: Deletes unused images. Like cleaning old photos.
- `docker system prune`: Big cleanup of everything. Like deep cleaning the house.

---

## Conclusion
In these five labs, I learned how containers start with ENTRYPOINT and CMD for flexible launches, how to build and deploy apps from GitHub for easy sharing and automation, why minimal images like Alpine are crucial for efficiency and speed, how tagging versions images for better management and updates, and how pruning cleans up Docker to save space and keep things organized. Overall, these labs taught me to use Docker powerfully for development, deployment, and maintenance, making me skilled in creating, sharing, and managing containerized applications. The hands-on experience with commands, GitHub integration, and best practices showed me that Docker isn't just for running apps—it's for building scalable, efficient, and collaborative software solutions. I also learned the importance of documentation and screenshots for tracking progress and sharing knowledge.