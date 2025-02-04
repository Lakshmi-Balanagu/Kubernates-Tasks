# Kubernetes Task: Deploy Grafana with NodePort Service

## 1. Create a Deployment for Grafana

- **Deployment Name**: `grafana-deployment`
- **Image**: Use any Grafana image (e.g., `grafana/grafana:latest`).
- **Replicas**: Set the number of replicas as per your choice (1 replica is sufficient for this task).
- **Port**: Expose port `3000` for Grafana, which is the default Grafana port.
- **Environment Variable**: Set `GF_SECURITY_ADMIN_PASSWORD` to `admin` to allow login to the Grafana dashboard with the default credentials.

### Parameters to Set:
- **Deployment Name**: grafana-deployment
- **Image**: grafana/grafana:latest (or any other valid Grafana image)
- **Replicas**: 1 (for simplicity)
- **Container Port**: 3000
- **Admin Password**: admin (default for testing, can be changed)

## 2. Create a NodePort Service to Expose the App

- **Service Type**: `NodePort`
- **NodePort**: `32000` (to expose the Grafana app on port 32000)
- **Target Port**: 3000 (the port where Grafana is running inside the container)
- **Port**: 3000 (the exposed service port)

This will expose the Grafana application on port `32000` of the nodes in the Kubernetes cluster, allowing access from outside the cluster.

---

With these steps, you should be able to deploy Grafana and expose it through the `NodePort` service to access it externally via `http://<Node_IP>:32000`.
