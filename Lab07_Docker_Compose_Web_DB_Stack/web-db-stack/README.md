# Lab 07: Docker Compose for a Web + DB Stack

## What You Will Learn (Easy Words)
- How to use Docker Compose to run more than one container at the same time.
- How to make a web server (nginx) and a database (MySQL) work together easily.
- How to keep your database data safe, even if you stop your containers.

## Key Topics (Explained Simply)
- **Docker Compose:** A tool that lets you start many containers with one command.
- **Services:** Each part of your app (like web or db) is called a service.
- **Volumes:** A way to save data from your containers so it doesn't get lost.
- **depends_on:** Makes sure the database starts before the web server.
- **Environment Variables:** Settings you give to containers (like database password).

---

## Step-by-Step Solution

### 1. Check Docker and Docker Compose
- To see if Docker is installed:
  ```
  docker --version
  ```
  (This shows your Docker version. If you see a version, Docker is ready.)
- To see if Docker Compose is installed:
  ```
  docker-compose --version
  ```
  (This shows your Docker Compose version.)

### 2. Make the Project Folder
- Run this to make a new folder and go into it:
  ```
  mkdir web-db-stack && cd web-db-stack
  ```
  (This makes a folder called web-db-stack and moves you into it.)

### 3. Make the docker-compose.yml File
- You can use Notepad or any editor, or run:
  ```
  touch docker-compose.yml
  ```
  (This makes a new file for your Docker Compose setup.)

### 4. Add the Services
- Copy this into docker-compose.yml:
  ```yaml
  version: '3.8'

  services:
    web:
      image: nginx:latest
      ports:
        - "80:80"
      depends_on:
        - db

    db:
      image: mysql:5.7
      environment:
        MYSQL_ROOT_PASSWORD: examplepassword
        MYSQL_DATABASE: exampledb
      volumes:
        - db_data:/var/lib/mysql

  volumes:
    db_data:
  ```
  (This sets up a web server and a database. The database saves its data in a special place so it doesn't get lost.)

### 5. Start Everything
- Run this to start both containers:
  ```
  docker-compose up -d
  ```
  (This starts your web and db containers in the background.)

### 6. Check if They Are Running
- See your running containers:
  ```
  docker-compose ps
  ```
  (This lists your web and db containers. If you see them, it's working!)
- Open your browser and go to [http://localhost](http://localhost) to see the nginx welcome page.

### 7. Test the Database
- Get into the MySQL container:
  ```
  docker-compose exec db mysql -u root -p
  ```
  (Type the password: examplepassword)
- Check the databases:
  ```
  SHOW DATABASES;
  ```
  (You should see exampledb in the list.)

### 8. Stop and Clean Up
- To stop and remove everything:
  ```
  docker-compose down
  ```
  (This stops and deletes the containers, but your database data is safe in the volume.)

---

## What Each Command Does (Easy Words)
- `docker --version`: Shows if Docker is installed.
- `docker-compose --version`: Shows if Docker Compose is installed.
- `mkdir web-db-stack && cd web-db-stack`: Makes a new folder and goes into it.
- `touch docker-compose.yml`: Makes a new file for your setup.
- `docker-compose up -d`: Starts all your containers in the background.
- `docker-compose ps`: Shows which containers are running.
- `docker-compose exec db mysql -u root -p`: Lets you use MySQL inside the db container.
- `SHOW DATABASES;`: Lists all databases in MySQL.
- `docker-compose down`: Stops and removes your containers.

---

## Conclusion
You learned how to use Docker Compose to run a web server and a database together, keep your data safe, and manage everything with simple commands!
