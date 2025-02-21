# **🔹 Docker Compose – A Complete Guide**  

## **1️⃣ What is Docker Compose?**  
Docker Compose is a tool that allows you to **define and manage multi-container Docker applications** using a simple YAML file (`docker-compose.yml`). Instead of running multiple `docker run` commands manually, Docker Compose helps **orchestrate services, networks, and volumes** in a single configuration file.  

✅ **Why Use Docker Compose?**  
- **Easier multi-container management** 🚀  
- **Single command to start multiple services** (`docker-compose up`)  
- **Automatic network creation** between services  
- **Environment variable management**  
- **Volume management** for persistent data  

---

## **2️⃣ Installation of Docker Compose**  
Docker Compose comes pre-installed with **Docker Desktop**, but if you're using **Linux**, install it using:  
```bash
sudo apt install docker-compose -y  # Ubuntu/Debian
```
Verify installation:  
```bash
docker-compose --version
```

---

## **3️⃣ Key Components of Docker Compose**
A `docker-compose.yml` file defines how services should run. The key components are:

| **Component** | **Description** |
|--------------|---------------|
| `services`   | Defines multiple containers (e.g., app, database, cache) |
| `image`      | Specifies the Docker image to use |
| `build`      | Defines how to build the image using a `Dockerfile` |
| `ports`      | Maps container ports to host machine ports |
| `volumes`    | Creates persistent storage for data |
| `environment` | Sets environment variables inside the container |
| `depends_on` | Defines service dependencies |

---

## **4️⃣ Writing a Simple `docker-compose.yml` File**
Let's create a **Node.js + MongoDB** application with Docker Compose.

### **Step 1: Create a `docker-compose.yml` File**
```yaml
version: "3.8"

services:
  app:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
    environment:
      - MONGO_URI=mongodb://db:27017/mydb
    volumes:
      - ./app:/usr/src/app

  db:
    image: mongo
    ports:
      - "27017:27017"
    volumes:
      - db-data:/data/db

volumes:
  db-data:
```

### **Explanation:**
- **`version: "3.8"`** → Specifies the Docker Compose file version.
- **`services`** → Defines two services: `app` (Node.js) and `db` (MongoDB).
- **`build: .`** → Builds the application using a `Dockerfile` in the current directory.
- **`ports`** → Maps container ports to host machine ports (`3000:3000` for the app, `27017:27017` for MongoDB).
- **`depends_on`** → Ensures `db` starts before `app`.
- **`volumes`** → Creates a **persistent volume** for MongoDB data.

---

## **5️⃣ Running a Docker Compose Project**
### **Step 1: Start the Services**
```bash
docker-compose up -d
```
➡️ This starts all the services in **detached mode (`-d`)**.

### **Step 2: Check Running Containers**
```bash
docker ps
```

### **Step 3: View Logs**
```bash
docker-compose logs -f
```

### **Step 4: Stop and Remove Services**
```bash
docker-compose down
```
➡️ This **removes all containers, networks, and volumes**.

---

## **6️⃣ Environment Variables in Docker Compose**
You can define environment variables in a **`.env` file** instead of hardcoding them.

🔹 **`.env` file:**
```
MONGO_USER=admin
MONGO_PASSWORD=secret
```

🔹 **Use it in `docker-compose.yml`:**
```yaml
environment:
  - MONGO_USER=${MONGO_USER}
  - MONGO_PASSWORD=${MONGO_PASSWORD}
```

---

## **7️⃣ Networks in Docker Compose**
By default, Docker Compose creates a **bridge network** for services to communicate.

✅ **Custom Network Example**
```yaml
networks:
  mynetwork:

services:
  app:
    networks:
      - mynetwork
  db:
    networks:
      - mynetwork
```
➡️ Both `app` and `db` are now in the same **custom network (`mynetwork`)**.

---

## **8️⃣ Volumes in Docker Compose**
Volumes persist data **even if the container stops**.

✅ **Example**
```yaml
volumes:
  db-data:

services:
  db:
    volumes:
      - db-data:/var/lib/mysql
```
➡️ This mounts a volume (`db-data`) inside `/var/lib/mysql`.

---

## **9️⃣ Scaling Services with Docker Compose**
You can run multiple instances of a service using **scaling**.

```bash
docker-compose up --scale app=3
```
➡️ Runs **3 replicas** of the `app` service.

---

## **🔟 Real-World Example: Deploying a Full Stack App**
Let's deploy a **React + Express + MongoDB** app.

### **Docker Compose File**
```yaml
version: "3.8"

services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend

  backend:
    build: ./backend
    ports:
      - "5000:5000"
    depends_on:
      - db
    environment:
      - DB_URI=mongodb://db:27017/mydatabase

  db:
    image: mongo
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```
➡️ This launches a **full stack app** with **React (frontend)**, **Node.js (backend)**, and **MongoDB**.

---

## **🔥 Summary**
| **Feature**  | **Description** |
|-------------|----------------|
| `docker-compose.yml` | Defines multi-container applications |
| `docker-compose up` | Starts services |
| `docker-compose down` | Stops and removes services |
| `depends_on` | Ensures dependencies start in order |
| `volumes` | Provides persistent storage |
| `networks` | Allows inter-container communication |
| `.env` | Stores environment variables |

---

## **🚀 Conclusion**
Docker Compose is an essential tool for **managing multi-container applications**. It simplifies development, testing, and deployment by using a **single YAML file**.  
