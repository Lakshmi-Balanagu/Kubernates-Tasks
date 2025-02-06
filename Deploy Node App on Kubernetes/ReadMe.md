# Task: Kubernetes Deployment and Service Setup

## Objective:
- Create a Kubernetes deployment using the `gcr.io/kodekloud/centos-ssh-enabled:node` image.
- Set the replica count to 2.
- Expose the deployment through a service with NodePort.

## Steps:

1. **Create a Deployment**  
   - Use the `gcr.io/kodekloud/centos-ssh-enabled:node` image.
   - Set the replica count to 2.

2. **Create a Service**  
   - Expose the application using a NodePort service.
   - Set the `targetPort` to 8080.
   - Set the `nodePort` to 30012.

3. **Ensure Pods are Running**  
   - Verify that all pods are in the Running state after the deployment.

4. **Access the Application**  
   
---

## Steps to Approach

### Step 1: Create a Deployment
1. Define a deployment for the application using the Docker image `gcr.io/kodekloud/centos-ssh-enabled:node`.
2. Set the replica count to `2` to ensure that two instances of the pod are running.
3. Expose the container port `8080` inside the deployment.

### Step 2: Create a Service to Expose the Application
1. Create a service to expose the application to the external network.
2. Set the service type as `NodePort`, which will expose the application on a specific port on each node in the cluster.
3. Set the target port of the service to `8080` to map to the application's container port.
4. Set the `nodePort` to `30012`, which is the port on which the service will be accessible externally.

### Step 3: Apply the Deployment and Service
1. Use `kubectl apply` to apply the deployment configuration to the Kubernetes cluster.
2. Apply the service configuration to expose the app externally.

### Step 4: Verify the Pods are in Running State
1. After deploying the application, check the status of the pods using the following command:
  ```bash
  kubectl get pods
  ```
2. Ensure that both pods are in the `Running` state. If they are not running, troubleshoot the issue by inspecting the pod logs and events.

### Step 5: Access the Application
1. Once the pods are in the `Running` state, you can access the application externally via the `NodePort`.
2. The application will be available at `http://<NodeIP>:30012`, where `<NodeIP>` is the IP address of any node in the cluster.

---

## Troubleshooting
- **Pod not in Running state:** If a pod is not in the `Running` state, use the following command to get more details:
  ```bash
  kubectl describe pod <pod-name>
  ```
