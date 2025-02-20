## **Docker Architecture – Detailed Explanation**  

Docker follows a **client-server architecture**, where the **Docker Client** interacts with the **Docker Daemon** to manage containers. It consists of various components such as the **Docker Engine, Docker Images, Containers, and Registries**.

---

## **🔹 Components of Docker Architecture**  

### **1️⃣ Docker Client 🖥️**  
- Acts as the interface to communicate with Docker.  
- Uses CLI commands like `docker build`, `docker run`, `docker pull`, etc.  
- Sends requests to the **Docker Daemon** using REST API, UNIX socket, or network.  

**Example Command:**  
```bash
docker run -d -p 80:80 nginx
```
🤚 This tells the **Docker Client** to request the **Docker Daemon** to run an Nginx container.  

---

### **2️⃣ Docker Daemon (dockerd) 🛠️**  
- Runs in the background and listens for **Docker Client** commands.  
- Manages images, containers, networks, and volumes.  
- Talks to the **Docker Registry** to pull or push images.  

---

### **3️⃣ Docker Images 📦**  
- A **read-only template** with application code, dependencies, and configurations.  
- Built using a **Dockerfile**.  
- Stored in **Docker Hub** or private registries.  

**Example of Creating an Image:**  
```dockerfile
FROM node:18-alpine  # Use a lightweight base image
WORKDIR /app         # Set working directory
COPY . .             # Copy all files
RUN npm install      # Install dependencies
EXPOSE 3000          # Expose application port
CMD ["node", "server.js"]  # Start the application
```

**Build the Image:**  
```bash
docker build -t my-app .
```

---

### **4️⃣ Docker Containers 🐳**  
- Running instances of **Docker Images**.  
- Uses **Container Runtime** (like `runc`) to start and manage containers.  
- Containers are **isolated environments** that share the **host OS kernel**.  

**Run a Container:**  
```bash
docker run -d -p 3000:3000 my-app
```
🤚 Runs the **my-app** container and maps port **3000**.

---

### **5️⃣ Docker Registry 📱**  
- Stores **Docker Images** for pulling and pushing.  
- **Public Registry**: Docker Hub.  
- **Private Registry**: AWS ECR, Azure Container Registry, etc.  

**Pull Image from Docker Hub:**  
```bash
docker pull nginx
```

**Push Image to Docker Hub:**  
```bash
docker tag my-app omjaju/my-app
docker push omjaju/my-app
```

---

## **🔹 How Docker Works – Step-by-Step Process**  

1️⃣ **Docker Client** sends a request to create a container.  
2️⃣ **Docker Daemon** receives the request and checks for the image locally.  
3️⃣ If the image is **not found**, it pulls it from the **Docker Registry**.  
4️⃣ The **Docker Daemon** creates a container from the image.  
5️⃣ The **Container Runtime** (like `runc`) starts the container in an **isolated environment**.  
6️⃣ The application runs inside the container.  

---

## **🔹 High-Level Diagram of Docker Architecture**  
```
+------------------+
| Docker Client   |  ---> Sends commands (docker run, build, etc.)
+------------------+
        |
        v
+------------------+
| Docker Daemon   |  ---> Manages images, containers, networks
+------------------+
        |
        v
+---------------------+       +---------------------+
| Docker Images      | <---> | Docker Registry     |  (Docker Hub, AWS ECR)
+---------------------+       +---------------------+
        |
        v
+---------------------+
| Docker Containers  |  ---> Running applications
+---------------------+
```

---

## **🔹 Why Use Docker?**  
👉 **Fast Deployment** – Containers start in seconds.  
👉 **Lightweight** – Uses less memory and resources than VMs.  
👉 **Portable** – Runs on any system with Docker installed.  
👉 **Scalable** – Easily handles multiple containers with **Docker Compose / Kubernetes**.  
