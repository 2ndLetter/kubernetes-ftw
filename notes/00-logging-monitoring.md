# ==Logging & Monitoring==
## Monitor Cluster Components:
- `git clone https://github.com/kubernetes-incubator/metrics-server.git`
- kubectl apply -f deploy/1.8+/
- `kubectl top pod`
- `kubectl top node`
## Managing Application Logs:
- `docker logs -f ecf`
- `kubectl top node`
- `kubectl logs -f webapp-1`
- `kubectl logs -f webapp-2 simple-webapp`
- If multiple containers within a Pod, you must provide the Pod and Container name