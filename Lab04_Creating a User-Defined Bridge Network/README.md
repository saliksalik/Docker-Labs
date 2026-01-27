# Lab 04: Creating a User-Defined Bridge Network

## Objective
- Understand the concept and benefits of user-defined bridge networks in Docker.
- Learn how to create a custom bridge network.
- Run Docker containers on a user-defined network to enable inter-container communication by name.
- Validate the network setup by testing connectivity between containers.

---

## Steps

### Step 1: Create a Custom Bridge Network
- **Command:**
  ```
  docker network create mynet
  ```
- **Screenshot:**
  ![Create a Custom Bridge Network](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab04_Creating%20a%20User-Defined%20Bridge%20Network/Outputs/Step%201%20Create%20a%20Custom%20Bridge%20Network.png)

### Step 2: Run Containers on the Custom Network
- **Command (container1):**
  ```
  docker run -d --name container1 --network mynet nginx
  ```
- **Screenshot:**
  ![Run Containers on the Custom Network](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab04_Creating%20a%20User-Defined%20Bridge%20Network/Outputs/Step%202%20Run%20Containers%20on%20the%20Custom%20Network.png)
- **Command (container2):**
  ```
  docker run -d --name container2 --network mynet nginx
  ```
- **Screenshot:**
  ![Run Containers on the Custom Network (container2)](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab04_Creating%20a%20User-Defined%20Bridge%20Network/Outputs/Step%202b%20Run%20Containers%20on%20the%20Custom%20Network.png)

### Step 3: Confirm Inter-Container Communication
- **Command:**
  ```
  docker exec -it container1 /bin/sh
  curl http://container2
  ```
- **Screenshot:**
  ![Confirm Inter-Container Communication](https://raw.githubusercontent.com/saliksalik/Docker-Labs/main/Lab04_Creating%20a%20User-Defined%20Bridge%20Network/Outputs/Step%203%20Confirm%20Inter-Container%20Communication.png)

---

## Key Learnings (Explained Simply)
- **User-Defined Bridge Network:** Like making your own private WiFi for containers. You control which containers join.
- **Container Name Communication:** On your custom network, containers can use each other’s names (like “container2”) to connect, not just numbers.
- **Why is this useful?** It makes things easier and more organized, especially when you have lots of containers.

---

## Conclusion
In this lab, I learned how to:
- Create a user-defined bridge network in Docker.
- Run containers on that network and communicate using container names.
- Validate the setup by testing connectivity between containers.

This approach simplifies container networks, enhances manageability, and adds flexibility to Docker deployments.
