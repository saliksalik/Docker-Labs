# Lab 11 | 12 | 13 | 14 | 15: Docker Lifecycle Hooks, GitHub Builds, Minimal Images, Tagging, and Cleanup

## What You Will Learn
- How Docker containers start and run with special commands like ENTRYPOINT and CMD.
- How to build and run Docker apps directly from code on GitHub.
- Why small images are better and how to use tiny base images like Alpine.
- How to name and version your Docker images properly.
- How to clean up old Docker stuff to save space.

## Key Concepts (Explained Simply)
- **ENTRYPOINT and CMD**: ENTRYPOINT is the main command that always runs when a container starts. CMD gives default extra words (arguments) to that command. Together, they decide what the container does first.
- **Building from GitHub**: You can put your app code and Dockerfile on GitHub, then clone it and build the Docker image locally. You can even auto-build with tools like GitHub Actions.
- **Minimal Base Images**: Instead of big images like Ubuntu, use tiny ones like Alpine (only 5MB) to make your containers small and fast.
- **Tagging and Versioning**: Give your images names with versions, like myapp:v1.0, so you know which one is which and can update safely.
- **Prune and Cleanup**: Docker can pile up old images and containers. Prune commands delete the unused ones to free up disk space.

---

## Step-by-Step Solution with Guidance

### Part 1: Docker Container Lifecycle Hooks (ENTRYPOINT and CMD)

#### Step 1: Create Project Folder
The lab asks to make a new folder for the project. So, I created `docker-lifecycle-hooks` and went inside it. This sets up a clean space for our files, and I learned that organizing folders helps keep projects separate.

#### Step 2: Create Dockerfile
Next, the lab wants a Dockerfile. I made a file called Dockerfile with this content:

```
FROM alpine:latest
ENTRYPOINT ["echo"]
CMD ["Hello, world!"]
```

This uses a tiny Alpine image. ENTRYPOINT sets "echo" as the command that always runs. CMD gives "Hello, world!" as the default message. I learned that ENTRYPOINT is like the boss command, and CMD is its helper.

#### Step 3: Build the Image
The lab says to build the image. I ran `docker build -t my-echo-app .`. This command takes the Dockerfile and makes a new image named my-echo-app. It downloads Alpine if needed and sets up the echo command. I learned that building creates the image from your instructions, like baking a cake from a recipe.

#### Step 4: Run the Container
Now, run it with `docker run my-echo-app`. The output was "Hello, world!" because ENTRYPOINT ran echo with CMD's default. This showed the container starting and doing its job. I learned that run starts the container, and it uses ENTRYPOINT + CMD together.

#### Step 5: Pass Arguments at Runtime
The lab asks to override CMD. I ran `docker run my-echo-app "Docker is awesome!"`. Now the output was "Docker is awesome!" instead of "Hello, world!". This happened because the new argument replaced CMD. I learned that you can change what the container says by adding words after the image name.

### Part 2: Building from GitHub in Docker

#### Step 6: Create GitHub Repo
The lab requires a GitHub repo. I went to GitHub, made a new public repo called docker-github-lab, and added a README. This is like a online folder for code. I learned that GitHub stores your projects safely online.

#### Step 7: Clone and Add App
Clone it locally with `git clone https://github.com/your-username/docker-github-lab.git` and cd into it. Then, create app.py with the Flask code. This is a simple web app that says "Hello, Docker!". I learned that cloning downloads the repo to your computer.

#### Step 8: Create Dockerfile
In the folder, make Dockerfile with:

```
FROM python:3.9-slim
WORKDIR /app
COPY . /app
RUN pip install flask
EXPOSE 5000
CMD ["python", "app.py"]
```

This uses Python image, copies files, installs Flask, exposes port 5000, and runs the app. I learned that Dockerfile tells Docker how to set up the app environment.

#### Step 9: Commit and Push
Add files with `git add .`, commit with `git commit -m "Add Flask app and Dockerfile"`, push with `git push origin main`. This uploads changes to GitHub. I learned that git tracks changes and shares them.

#### Step 10: Build and Run Locally
Build with `docker build -t my-flask-app .`. This makes the image from the Dockerfile. Then run with `docker run -p 5000:5000 my-flask-app`. The -p maps port 5000 so you can visit http://localhost:5000. I learned that building assembles the image, and running starts the web app.

#### Step 11: Add GitHub Actions (Optional)
Create .github/workflows/docker-image.yml with the workflow code. This auto-builds the image on pushes. Commit and push it. I learned that GitHub Actions automates tasks like building images.

### Part 3: Minimal Base Images

#### Step 12: Create Alpine Dockerfile
Make folder `minimal-base-image`, cd in, create Dockerfile with:

```
FROM alpine:latest
RUN apk add --no-cache curl bash
```

Alpine is super small. apk is its installer, --no-cache skips extras to stay tiny. I learned that Alpine keeps images lightweight.

#### Step 13: Build Alpine Image
Run `docker build -t my-alpine-image .`. This builds the small image. I learned that building with Alpine makes tiny containers.

#### Step 14: Create Ubuntu Dockerfile
Make Dockerfile.ubuntu with:

```
FROM ubuntu:latest
RUN apt-get update && apt-get install -y curl bash
```

Ubuntu is bigger. apt-get installs packages. I learned that Ubuntu is familiar but heavy.

#### Step 15: Build Ubuntu Image
Run `docker build -f Dockerfile.ubuntu -t my-ubuntu-image .`. The -f uses the other Dockerfile. I learned that you can have multiple Dockerfiles.

#### Step 16: Compare Sizes
Run `docker images` to see sizes. Alpine is much smaller (like 5MB vs 100MB+). I learned that small images load faster and use less space.

### Part 4: Docker Tagging and Versioning

#### Step 17: Create App and Dockerfile
Make a folder, create requirements.txt (with flask), app.py (simple Flask), and Dockerfile as given. I learned that requirements.txt lists needed packages.

#### Step 18: Build with Tag
Run `docker build -t myapp:v1 .`. The -t adds the tag v1. I learned that tags version your images.

#### Step 19: Login and Push (Optional)
Run `docker login` to connect to Docker Hub. Then `docker tag myapp:v1 username/myapp:v1` to rename for Hub. Push with `docker push username/myapp:v1`. I learned that pushing shares images online.

#### Step 20: Best Practices
Use lowercase names, semantic versions like 1.0.0, or dates. Avoid overusing "latest". I learned that good naming prevents mix-ups.

### Part 5: Docker Prune & Cleanup

#### Step 21: List Images and Containers
Run `docker images` to see all images. Run `docker ps -a` to see all containers. I learned that listing shows what's there.

#### Step 22: Prune Images
Run `docker image prune` for dangling images (untagged). Or `docker image prune -a` for all unused. I learned that prune deletes junk to save space.

#### Step 23: Prune Containers and Volumes
Run `docker container prune` for stopped containers. `docker volume prune` for unused volumes. I learned that these clean up leftovers.

#### Step 24: System Prune
Run `docker system prune` for everything. Or `docker system prune --all` including networks. I learned that system prune is a big cleanup.

---

## What Each Command Does (Easy Words)
- `mkdir folder && cd folder`: Makes a new folder and goes inside it. Like creating a room for your stuff.
- `touch file`: Creates an empty file. Like starting a blank page.
- `docker build -t name .`: Builds an image from Dockerfile with a name. Like cooking a meal from a recipe.
- `docker run image`: Starts a container from the image. Like turning on a machine.
- `docker run image "text"`: Runs with custom text, overriding defaults. Like changing the machine's message.
- `git clone url`: Downloads a GitHub repo to your computer. Like copying a book from the library.
- `git add . && git commit -m "msg" && git push`: Saves changes and uploads to GitHub. Like saving and sharing your work.
- `docker run -p 5000:5000 image`: Runs and connects port 5000. Like opening a door to the app.
- `apk add --no-cache pkg`: Installs packages in Alpine without cache. Like adding light tools.
- `apt-get update && apt-get install pkg`: Updates and installs in Ubuntu. Like shopping for tools.
- `docker images`: Lists all images with sizes. Like checking your photo album.
- `docker build -t name:tag .`: Builds with a version tag. Like labeling a photo.
- `docker login && docker push name`: Logs in and uploads image. Like posting a photo online.
- `docker ps -a`: Lists all containers. Like seeing all your running apps.
- `docker image prune`: Deletes unused images. Like cleaning old photos.
- `docker system prune`: Big cleanup of everything. Like deep cleaning the house.

---

## Conclusion
In these labs, I learned how containers start with ENTRYPOINT and CMD, how to build apps from GitHub code, why tiny images like Alpine are great, how to tag images for versions, and how to clean up Docker junk. This makes Docker easier to use, faster, and less messy. I can now make efficient, versioned containers and keep my system tidy. These skills help in real projects for better deployments and teamwork.