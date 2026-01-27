# Lab 05: Linking Containers

## Objective
- Understand the concept of linking containers in Docker.
- Learn how to use the --link flag to connect two containers.
- Compare container linking with user-defined networks.
- Identify drawbacks of container linking and understand why user-defined networks are recommended.

---

## Steps

### Step 1: Pull Required Docker Images
- **Command:**
  ```
  docker pull httpd
  docker pull mysql
  ```
- **Screenshot:**
  ![Pulling Required Docker Images](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab05_Linking_Containers/Outputs/Step%201%20Pulling%20Required%20Docker%20Images.png)

### Step 2: Start the Database Container
- **Command:**
  ```
  docker run --name mydb -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql
  ```
- **Screenshot:**
  ![Start the Database Container](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab05_Linking_Containers/Outputs/Step%202%20Start%20the%20Database%20Container.png)

### Step 3: Link the Web Server to the Database
- **Command:**
  ```
  docker run --name myweb --link mydb:mysql -d httpd
  ```
- **Screenshot:**
  ![Link the Web Server to the Database](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab05_Linking_Containers/Outputs/Step%203%20Link%20the%20Web%20Server%20to%20the%20Database.png)

### Step 4: Run Containers Using the Network (Recommended Way)
- **Command:**
  ```
  docker network create mynetwork
  docker run --name mydb --network mynetwork -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql
  docker run --name myweb --network mynetwork -d httpd
  ```
- **Screenshot:**
  ![Run Containers Using the Network](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab05_Linking_Containers/Outputs/Step%204%20Run%20Containers%20Using%20the%20Network.png)

---

## Key Learnings (Explained Simply)
- **Container Linking:** Like making a shortcut so one container can find another by a nickname. But it’s old and not recommended anymore.
- **User-Defined Networks:** Like making your own private WiFi for containers, where they can all talk to each other easily.
- **Why not use --link?** It’s old, limited, and not recommended anymore. Networks are better for teamwork!

---

## Conclusion
In this lab, I learned how to:
- Link containers using the deprecated --link approach.
- Use user-defined networks for efficient container communication.
- Understand why linking is replaced by newer and more robust Docker networking features.

Embracing Docker networks is essential for modern container orchestration, fostering better service scalability, isolation, and dynamic configuration.
