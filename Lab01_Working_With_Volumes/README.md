# Lab 01: Working with Volumes

## Objective
Learn how Docker volumes work and their importance in data persistence within containers.

## Steps

### Step 1: Create a Named Volume
- **Command:**
  ```
  docker volume create myvolume
  ```
- **Screenshot:**
  ![Create Volume Output](Outputs/image%202%20docker%20desktop%20contianer%20made.png)

### Step 2: Run a Container Using the Volume
- **Command:**
  ```
  docker run --name volcontainer -v myvolume:/data -d alpine sh -c "echo 'Hello World' > /data/hello.txt"
  ```
- **Explanation:**
  - Creates a container named `volcontainer`.
  - Mounts the volume `myvolume` to `/data` inside the container.
  - Writes "Hello World" to a file named `hello.txt` in the volume.
- **Screenshot:**
  ![Making Container](Outputs/Making%20container%20.png)

### Step 3: Inspect the Volume Data
- **Command:**
  ```
  docker run --rm -v myvolume:/data alpine cat /data/hello.txt
  ```
- **Explanation:**
  - Runs a temporary container to read the contents of `hello.txt`.
- **Screenshot:**
  ![Inspect the Volume Data](Outputs/image%203(inspect%20the%20volume%20data).png)

### Step 4: Remove the Container
- **Command:**
  ```
  docker rm -f volcontainer
  ```
- **Explanation:**
  - Removes the container named `volcontainer`.
- **Screenshot:**
  ![Remove the Container](Outputs/image%204%20remove%20the%20container.png)

### Step 5: Confirm Data Persistence
- **Command:**
  ```
  docker run --rm -v myvolume:/data alpine cat /data/hello.txt
  ```
- **Explanation:**
  - Verifies that the file `hello.txt` still exists in the volume after the container is removed.
- **Screenshot:**
  ![Confirm Data Persistence](Outputs/image%203(inspect%20the%20volume%20data).png)

### Step 6: Clean Up (Optional)
- **Command:**
  ```
  docker volume rm myvolume
  ```
- **Explanation:**
  - Deletes the volume `myvolume`.
- **Screenshot:**
  ![Cleanup Volume](Outputs/image%205%20cleanup%20volume.png)

## Key Learnings
- Docker volumes persist data beyond the lifecycle of containers.
- Volumes are essential for applications requiring persistent data.

## Resources
- [Docker Documentation](https://docs.docker.com/)