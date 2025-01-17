### **1. Introduction to Docker Networking**
#### **Description**:
Docker networking is the communication framework that allows containers to interact with each other, the host system, and external systems. Networking plays a crucial role in ensuring containerized applications work seamlessly.

#### **Concept**:
- Docker provides a built-in networking model with different types of networks like `bridge`, `host`, and `overlay`.
- Containers are assigned their own network namespace with isolated virtual network interfaces.
  
#### **Diagram**:
- Diagram showing:
  - Containers connected to a `bridge` network.
  - Traffic flow between containers and the host.
  
#### **Example**:
```bash
# Inspect the default bridge network
docker network inspect bridge
```

---

### **2. Docker Networking Commands**
#### **Description**:
Docker provides several commands to manage networks.

#### **Concept**:
- `docker network ls`: Lists available networks.
- `docker network create`: Creates a new network.
- `docker network connect`: Connects a container to a network.
- `docker network disconnect`: Disconnects a container from a network.

#### **Example**:
```bash
# Create a custom network
docker network create my_custom_network

# Connect a container to the custom network
docker run -dit --name my_container --network my_custom_network nginx

# List networks
docker network ls
```

#### **Diagram**:
Flowchart of a container being connected/disconnected from a custom network.

---

### **3. Types of Docker Networks**
#### **Description**:
Docker supports several network types tailored for different use cases.

#### **Concept**:
1. **Bridge Network**:
   - Default network for standalone containers.
   - Containers on the same bridge network can communicate.
2. **Host Network**:
   - Shares the host’s network stack.
   - No isolation.
3. **Overlay Network**:
   - Used for multi-host communication in Swarm mode.
4. **Macvlan Network**:
   - Assigns a MAC address to the container, making it appear as a physical device.
5. **None Network**:
   - No network interface attached; completely isolated.

#### **Diagram**:
Diagram showing different networks and their use cases.

#### **Example**:
```bash
# Create an overlay network
docker network create -d overlay my_overlay_network
```

### **4. Exposing Containers Externally**
#### **Description**:
Exposing containers allows services inside containers to be accessible outside the host.

#### **Concept**:
- Use **port mapping** to expose a container.
- Syntax: `-p HOST_PORT:CONTAINER_PORT`.
  
#### **Diagram**:
Illustration of port mapping, showing requests from external users hitting the host’s port and being forwarded to the container.

#### **Example**:
```bash
# Run an Nginx container and expose it on port 8080
docker run -d -p 8080:80 nginx
```

### **5. Network Troubleshooting**
#### **Description**:
Troubleshooting helps identify issues in container communication.

#### **Concept**:
- Common Issues:
  - Misconfigured network.
  - Containers not on the same network.
  - DNS issues.
- Tools:
  - `docker network inspect`: Inspect network configurations.
  - `ping`: Check connectivity between containers.
  - `docker logs`: Check container logs for network errors.

#### **Diagram**:
Troubleshooting workflow with commands and tools.

#### **Example**:
```bash
# Check the network of a container
docker inspect <container_name> | grep NetworkSettings

# Ping another container
docker exec <container1_name> ping <container2_name>
```

---

### **6. Configuring Docker to Use External DNS**
#### **Description**:
Docker containers often need to resolve domain names. Configuring an external DNS ensures they can access external services.

#### **Concept**:
- DNS configurations are specified in `/etc/docker/daemon.json`.
- Example configuration:
  ```json
  {
    "dns": ["8.8.8.8", "8.8.4.4"]
  }
  ```

#### **Diagram**:
Diagram showing container DNS resolution flow to external servers.

#### **Example**:
```bash
# Restart Docker after DNS changes
sudo systemctl restart docker
```
