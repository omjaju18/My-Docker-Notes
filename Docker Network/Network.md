# **Docker Networking in Detail**

Docker networking allows containers to communicate with each other and external systems efficiently. Understanding Docker networking is crucial for designing scalable and secure containerized applications.

## **Types of Docker Networks**

### 1. **Bridge Network (Default)**  
- Default network for containers when no network is specified.
- Allows communication between containers on the same host using NAT (Network Address Translation).
- Containers can only communicate using IP addresses unless explicitly linked.

### 2. **Custom Bridge Network**  
- A user-defined bridge network with improved control over container communication.
- Enables automatic name-based resolution between containers.
- Recommended over the default bridge network for better isolation and security.

### 3. **Host Network**  
- The container shares the host machine’s network stack.
- No separate IP address for the container; it uses the host’s networking.
- Offers better performance but lacks isolation.

### 4. **None Network**  
- Disables all network access for a container.
- Provides the highest level of isolation.
- Suitable for security-sensitive applications.

### 5. **Overlay Network**  
- Enables communication between containers running on different Docker hosts.
- Used in Docker Swarm for distributed applications.
- Enhances security and efficient networking across multiple nodes.

### 6. **Macvlan Network**  
- Assigns a unique MAC address to each container, making them appear as separate physical devices.
- Allows direct communication with external networks.
- Useful when containers need to behave like real network devices.

## **Docker Network Commands**

To **list available networks**, use:
```bash
docker network ls
```

To **inspect a specific network**, use:
```bash
docker network inspect <network_name>
```

To **create a custom bridge network**, use:
```bash
docker network create -d bridge my-custom-network
```

To **run a container inside a custom bridge network**, use:
```bash
docker run -dit --name container1 --network my-custom-network nginx
docker run -dit --name container2 --network my-custom-network ubuntu
```

To **connect a running container to another network**, use:
```bash
docker network connect my-custom-network container1
```

To **disconnect a container from a network**, use:
```bash
docker network disconnect my-custom-network container1
```

To **remove a custom network**, use:
```bash
docker network rm my-custom-network
```

To **delete all unused networks**, use:
```bash
docker network prune
```

## **Custom Bridge Network Example**

A **custom bridge network** allows better control over container communication.

### **Steps to Create a Custom Bridge Network**

1. **Create a Custom Bridge Network**
```bash
docker network create -d bridge my-bridge-network
```

2. **Run Containers in the Custom Network**
```bash
docker run -dit --name container1 --network my-bridge-network nginx
docker run -dit --name container2 --network my-bridge-network ubuntu
```

3. **Verify the Network**
```bash
docker network inspect my-bridge-network
```

4. **Testing Communication**
- Enter container2 and ping container1 by name:
```bash
docker exec -it container2 ping container1
```

## **Key Differences: Default Bridge vs. Custom Bridge**

| Feature | Default Bridge | Custom Bridge |
|---------|--------------|---------------|
| DNS-Based Communication | ❌ No | ✅ Yes |
| Isolation | Moderate | Better control |
| Manual Linking Required? | ✅ Yes | ❌ No |

## **Conclusion**  
Using **custom bridge networks** is the recommended approach for better container communication and security. **Host networks should be avoided in production** due to their lack of isolation. **Overlay and Macvlan networks** are useful for advanced use cases like multi-host deployments and direct network communication.

