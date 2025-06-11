# -----------------------------
# Kubernetes Application Troubleshooting Cheat Sheet
# -----------------------------

### 🚦 1. Check Pod Status
```bash
kubectl get pods -n <namespace>
```
Common issues:
- `CrashLoopBackOff`
- `ImagePullBackOff`
- `Pending`

### 📝 2. Describe the Pod (Detailed Info)
```bash
kubectl describe pod <pod-name> -n <namespace>
```
- Look at Events section for issues
- Check resource allocation, scheduling, probes

### 📄 3. View Logs
```bash
kubectl logs <pod-name> -n <namespace>
kubectl logs -f <pod-name> -n <namespace>
kubectl logs <pod-name> -c <container-name>  # For multi-container pods
```

### 🔁 4. Restart the Pod
```bash
kubectl delete pod <pod-name> -n <namespace>
kubectl rollout restart deployment <deployment-name> -n <namespace>
```

### 🔍 5. Check Deployment
```bash
kubectl get deployment <deployment-name> -n <namespace>
kubectl describe deployment <deployment-name> -n <namespace>
```

### 🧪 6. Inspect Services & Port Mappings
```bash
kubectl get svc -n <namespace>
kubectl describe svc <service-name> -n <namespace>
```
- Confirm `targetPort`, `port`, and `nodePort`

### 🌐 7. Debug Networking (from inside cluster)
```bash
kubectl run -it busybox --image=busybox --restart=Never -- sh
wget <service-name>:<port>
```

### 🔒 8. Check ConfigMaps and Secrets
```bash
kubectl get configmap,secret -n <namespace>
kubectl describe configmap <name> -n <namespace>
kubectl describe secret <name> -n <namespace>
```

### 🧠 9. Review Liveness/Readiness Probes
```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 15
  periodSeconds: 10
```

### 📈 10. Use Metrics or Dashboards
```bash
kubectl top pod -n <namespace>
```
(Ensure Metrics Server is installed)
- Use Prometheus, Grafana, or K9s for deeper insight

---

### 🧪 Common Real-Time Issues + Fixes

#### ❌ ImagePullBackOff
**Cause:** Wrong image name or unauthenticated private registry
```bash
kubectl describe pod <pod>  # Look at Events
```
✅ **Fix:** Update Deployment YAML with correct image or secret
```yaml
image: nginx:latest
imagePullSecrets:
  - name: my-dockerhub-secret
```

#### ❌ CrashLoopBackOff
**Cause:** App crashes due to config/env/port mismatch
```bash
kubectl logs <pod>
```
✅ **Fix:** Check env vars, ConfigMaps, secret references

#### ❌ Liveness/Readiness Failures
**Cause:** Probe paths incorrect or service delay
✅ **Fix:** Update probe to match actual app health endpoint
```yaml
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5
```

---

### 🤖 Add GitHub Actions for Health Check
```yaml
# .github/workflows/k8s-healthcheck.yml
name: K8s Health Check
on:
  schedule:
    - cron: '*/30 * * * *'

jobs:
  probe:
    runs-on: ubuntu-latest
    steps:
    - name: Curl endpoint
      run: |
        curl --fail http://my-app.example.com/healthz || exit 1
```

---

### 🔔 Alerting with Prometheus
**Steps:**
1. Install Prometheus + Alertmanager in cluster
2. Configure alert rules:
```yaml
- alert: PodCrashLooping
  expr: kube_pod_container_status_restarts_total > 5
  for: 2m
  labels:
    severity: critical
  annotations:
    summary: "Pod is crash looping"
```
3. Integrate with Slack, Email, OpsGenie

# -----------------------------
# End of Kubernetes Troubleshooting Cheat Sheet
# -----------------------------
