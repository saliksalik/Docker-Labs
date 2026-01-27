# Lab 03: Docker Networking Basics

## Objective
- Understand the basics of Docker networking.
- Learn how to connect Docker containers on the same network.
- Use Docker CLI commands to explore and manipulate container networks.

---

## Steps

### Step 1: Pull the Alpine Image
- **Command:**
  ```
  docker pull alpine
  ```
- **Screenshot:**
  ![Pull the Alpine Image](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab03_Docker_Networking/Outputs/Step%201%20Pull%20the%20Alpine%20Image.png)

### Step 2: Run Two Containers
- **Command:**
  ```
  docker run -d --name container1 alpine sleep 1000
  docker run -d --name container2 alpine sleep 1000
  ```
- **Screenshot:**
  ![Run Two Containers](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab03_Docker_Networking/Outputs/Step%202%20Run%20Two%20Containers.png)

### Step 3: Find Each Container’s IP Address
- **Command:**
  ```
  docker inspect container1
  docker inspect container2
  ```
- **Screenshot (container1):**
  ![Find Container1's IP Address](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab03_Docker_Networking/Outputs/Step%20(3a)Find%20Container1's%20IP%20Address.png)
- **Screenshot (container2):**
  ![Find Container2's IP Address](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab03_Docker_Networking/Outputs/Step%203b%20Find%20%20Container2's%20IP%20Address.png)

### Step 4: Install Ping Utility in Both Containers
- **Command:**
  ```
  docker exec container1 apk add --no-cache iputils
  docker exec container2 apk add --no-cache iputils
  ```

### Step 5: Ping from One Container to the Other
- **Command:**
  ```
  docker exec container1 ping -c 4 [IP_ADDRESS_OF_CONTAINER2]
  ```
  Replace `[IP_ADDRESS_OF_CONTAINER2]` with the actual IP address you found for container2.
- **Screenshot:**
  ![Ping from One Container to the Other](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab03_Docker_Networking/Outputs/Step%205%20Ping%20from%20One%20Container%20to%20the%20Other.png)

### Step 6: Clean Up
- **Command:**
  ```
  docker rm -f container1 container2
  ```
- **Screenshot:**
  ![Clean Up](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab03_Docker_Networking/Outputs/Step%206%20Clean%20Up.png)

---

## Key Learnings (Explained Simply)
- **Docker Network:** Like a WiFi for containers. Containers on the same network can talk to each other.
- **Bridge Network:** Docker’s default network. If you don’t make a special network, containers use this one.
- **IP Address:** Every container gets its own “house address” so others can find it.
- **Ping:** A way to check if one container can “see” another container (like saying “hello, are you there?”).

---

## Teaching Tips
- Show each screenshot after running the command so learners can see what to expect.
- Explain each step in simple words, as if you’re teaching someone new to Docker.
- Remind learners to clean up containers when done to keep things tidy.

---

## Conclusion
In this lab, you learned how to:
- Launch containers and connect them on the default bridge network.
- Use `docker inspect` to find network information.
- Verify inter-container communication using `ping`.

These basics are the foundation for more advanced Docker networking!
