# Lab 02: Bind Mounts vs. Named Volumes

## Objective
- Understand the differences between bind mounts and named volumes in Docker.
- Learn how to create and use bind mounts and named volumes.
- Identify scenarios where each storage type is most appropriate.

---

## Steps

### Part 1: Bind Mounts

#### Step 1: Create a Bind Mount Directory and File
- **Command:**
  ```
  mkdir "C:\Users\Dell\Desktop\Docker LABS\Lab02_Bind_Mounts_vs_Named_Volumes\host-directory"
  echo Hello from the host! > "C:\Users\Dell\Desktop\Docker LABS\Lab02_Bind_Mounts_vs_Named_Volumes\host-directory\host-file.txt"
  ```
- **Screenshot:**
  ![Bind Mount Directory and File](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab02_Bind_Mounts_vs_Named_Volumes/Outputs/bind-mount-directory-and-file.png)

#### Step 2: Run Nginx Container with Bind Mount
- **Command:**
  ```
  docker run -d --name nginx-bind -v "C:\Users\Dell\Desktop\Docker LABS\Lab02_Bind_Mounts_vs_Named_Volumes\host-directory:/usr/share/nginx/html:ro" -p 8080:80 nginx
  ```
- **Screenshot:**
  ![Nginx Bind Container](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab02_Bind_Mounts_vs_Named_Volumes/Outputs/nginx-bind-container.png)

#### Step 3: Verify Bind Mount
- **Command:**
  ```
  curl http://localhost:8080/host-file.txt
  ```
- **Screenshot:**
  ![Bind Mount Curl Output](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab02_Bind_Mounts_vs_Named_Volumes/Outputs/bind-mount-curl-output.png)

---

### Part 2: Named Volumes

#### Step 4: Create a Named Volume
- **Command:**
  ```
  docker volume create my-volume
  ```
- **Screenshot:**
  ![Create Named Volume](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab02_Bind_Mounts_vs_Named_Volumes/Outputs/create-named-volume.png)

#### Step 5: Run Nginx Container with Named Volume
- **Command:**
  ```
  docker run -d --name nginx-volume -v my-volume:/usr/share/nginx/html -p 8081:80 nginx
  ```
- **Screenshot:**
  ![Nginx Volume Container](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab02_Bind_Mounts_vs_Named_Volumes/Outputs/nginx-volume-container.png)

#### Step 6: Copy File into Named Volume
- **Command:**
  ```
  docker cp "C:\Users\Dell\Desktop\Docker LABS\Lab02_Bind_Mounts_vs_Named_Volumes\host-directory\host-file.txt" nginx-volume:/usr/share/nginx/html/
  ```
- **Screenshot:**
  ![Copy File to Named Volume](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab02_Bind_Mounts_vs_Named_Volumes/Outputs/copy-file-to-named-volume.png.png)

#### Step 7: Verify Named Volume
- **Command:**
  ```
  curl http://localhost:8081/host-file.txt
  ```
- **Screenshot:**
  ![Named Volume Curl Output](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab02_Bind_Mounts_vs_Named_Volumes/Outputs/named-volume-curl-output.png)

---

## Comparison and Use Cases

- **Bind Mounts:**
  - Useful for development, as changes on the host are instantly reflected in the container.
  - Example: Sharing source code between your computer and a running container.
- **Named Volumes:**
  - Managed by Docker, good for production and data persistence.
  - Example: Storing database files or application data that should not be affected by host changes.

---

## Key Learnings
- Bind mounts are best for development and real-time file sharing.
- Named volumes are best for persistent, managed storage in production.

---


---

## Notes
- **Bind Mount:** Links a folder from your computer to the container. Any change you make on your computer is instantly visible in the container. Great for development.
- **Named Volume:** Managed by Docker. Data is stored in a special Docker folder, not directly on your computer. Good for production and when you want your data to be safe from accidental changes on your computer.
- **Remember:**
  - Use bind mounts for fast, live updates (like coding).
  - Use named volumes for safe, persistent storage (like databases).
