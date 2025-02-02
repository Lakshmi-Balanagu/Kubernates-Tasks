# Kubernetes Task: Deployment, Update, and Rollback

1. Create a namespace called `nautilus`.
2. Create a deployment named `httpd-deploy` in the `nautilus` namespace with the following specifications:
   - A container named `httpd`
   - The container should use the `httpd:2.4.25` image
   - The deployment should have 4 replicas
   - The deployment should use the `RollingUpdate` strategy with `maxSurge=1` and `maxUnavailable=2`.
3. Create a `NodePort` service named `httpd-service` that exposes the deployment on `nodePort: 30008`.
4. Upgrade the deployment to use the `httpd:2.4.43` image using a rolling update.
5. Once all pods are updated, roll back to the previous version (`httpd:2.4.25`).

---

# Process

1. Create the `nautilus` namespace.
   ```sh
   kubectl create namespace nautilus
   ```
2. Create the `httpd-deploy` deployment in the `nautilus` namespace with the specified settings.  
   Save the following YAML to a file named `httpd-deploy.yaml`.
   ```sh
   kubectl apply -f httpd-deploy.yaml
   ```
3. Create a `httpd-service` NodePort type service.  
   Save the following YAML to a file named `httpd-service.yaml`.
   ```sh
   kubectl apply -f httpd-service.yaml
   ```
4. Upgrade the deployment to `httpd:2.4.43` using a rolling update.  
   - Option 1: Use the following command.
     ```sh
     kubectl set image deployment/httpd-deploy httpd=httpd:2.4.43 -n nautilus
     ```
   - Option 2: Manually update the `httpd-deploy.yaml` file.
5. Rollback to the previous version after all pods are updated.
   ```sh
   kubectl rollout undo deployment/httpd-deploy -n nautilus
   ```
   
---
   
# situation
   
   when you’re tasked with deploying a new version of a web application or service in a production environment while minimizing downtime and ensuring smooth traffic routing during updates. 
