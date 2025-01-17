### **Assessment and Q&A (20 minutes)**

#### **Quiz: Multiple-Choice Questions**

Here is a set of **multiple-choice questions (MCQs)** to test students’ understanding of Docker networking:

---

1. **What is the default network type used when you start a Docker container without specifying a network?**  
   A. Host  
   B. Bridge  
   C. None  
   D. Overlay  
   **Answer**: B. Bridge  

---

2. **Which command is used to list all Docker networks on the host machine?**  
   A. `docker network ls`  
   B. `docker network show`  
   C. `docker networks`  
   D. `docker network inspect`  
   **Answer**: A. `docker network ls`  

---

3. **Which type of Docker network is best suited for multi-host communication in Swarm mode?**  
   A. Host  
   B. Bridge  
   C. Overlay  
   D. Macvlan  
   **Answer**: C. Overlay  

---

4. **What does the `-p` flag do when running a Docker container?**  
   A. Links two containers  
   B. Exposes container ports externally  
   C. Assigns a custom IP address  
   D. Connects to a custom network  
   **Answer**: B. Exposes container ports externally  

---

5. **Which command is used to troubleshoot a Docker network configuration?**  
   A. `docker network check`  
   B. `docker inspect`  
   C. `docker network inspect`  
   D. `docker troubleshoot`  
   **Answer**: C. `docker network inspect`  

---

6. **To configure Docker to use external DNS servers, you need to edit which file?**  
   A. `/etc/docker/config.json`  
   B. `/etc/docker/daemon.json`  
   C. `/etc/resolv.conf`  
   D. `/etc/network/interfaces`  
   **Answer**: B. `/etc/docker/daemon.json`  

---

7. **What happens if you use the `--network none` option while running a container?**  
   A. The container will not have any network access.  
   B. The container will use the host network.  
   C. The container will use a custom network.  
   D. The container will use the default bridge network.  
   **Answer**: A. The container will not have any network access.  

---

#### **Practical Task**

**Objective**: Test students' ability to create and manage Docker networks, launch containers, and expose them externally.  

---

**Steps**:  
1. **Create a custom Docker network**  
   ```bash
   docker network create student_network
   ```

2. **Launch two containers connected to the custom network**  
   ```bash
   docker run -dit --name web_app1 --network student_network nginx
   docker run -dit --name web_app2 --network student_network nginx
   ```

3. **Expose one of the containers externally using port mapping**  
   ```bash
   docker run -dit --name web_app_external -p 8080:80 --network student_network nginx
   ```

4. **Verify the network connectivity between containers**  
   - Use the `docker exec` command to ping from one container to another:  
     ```bash
     docker exec -it web_app1 ping web_app2
     ```

5. **Access the externally exposed container**  
   - Open a web browser and navigate to `http://localhost:8080` to check if the Nginx service is accessible.  

---

#### **Evaluation Criteria**
- Did the student successfully create a custom Docker network?  
- Were the containers correctly connected to the network?  
- Could the student troubleshoot network connectivity issues (if any)?  
- Was the exposed container accessible via the browser?  
