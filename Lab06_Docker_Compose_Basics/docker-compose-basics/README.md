# Lab 06: Docker Compose Basics

## Objective
Learn how to use Docker Compose to manage multi-container applications with a simple example.

---

## Lab Structure
- **Directory:** Lab06_Docker_Compose_Basics/docker-compose-basics/
- **Key File:** docker-compose.yml

---

## Step 1: Create the Directory Structure

```
Lab06_Docker_Compose_Basics/
└── docker-compose-basics/
    └── docker-compose.yml
```

---

## Step 2: Create docker-compose.yml

**File:** docker-compose.yml

```
version: '3'
services:
  web:
    image: nginx:latest
    ports:
      - "8081:80"
  app:
    image: alpine
    command: ["echo", "Hello, Docker Compose!"]
```

---

## Step 3: Run Docker Compose

Open a terminal in the `docker-compose-basics` directory and run:

```
docker-compose up
```

- This command will start two services:
  - **web:** Runs an nginx web server, accessible at http://localhost:8081
  - **app:** Runs a one-off Alpine Linux container that prints "Hello, Docker Compose!" and exits.

---

## Step 4: Check Running Containers

To see running containers:

```
docker ps -a
```

---

## Step 5: Stop and Remove Containers

To stop and remove the containers and network created by Docker Compose:

```
docker-compose down
```

---

## Troubleshooting
- Ensure you are in the correct directory containing `docker-compose.yml` when running commands.
- If containers do not start, check for typos in the file name or YAML syntax.
- Use `docker-compose config` to validate your configuration.
- Use `docker-compose --version` to check if Docker Compose is installed.

---

## Outputs
- The nginx web server should be accessible at [http://localhost:8081](http://localhost:8081).
- The Alpine container will print a message and exit (check with `docker ps -a`).

---

## Screenshots
Add screenshots of your terminal output and browser (nginx welcome page) here for documentation.

---

## Notes
- This lab demonstrates the basics of Docker Compose for orchestrating multi-container applications.
- For more complex setups, you can add more services, networks, and volumes in the `docker-compose.yml` file.

---

## References
- [Docker Compose Documentation](https://docs.docker.com/compose/)
