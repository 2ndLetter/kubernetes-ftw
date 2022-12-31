## DaemonSet Example: [docs](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: myapp-daemon
spec:
  selector:
    matchLabels:
      app: monitoring-agent # ENSURE THIS LABEL MATCHES THE POD TEMPLATE
  template:
    metadata:
      labels:
        app: monitoring-agent
    spec:
      containers:
      - name: monitoring-agent
        image: monitoring-agent  
```
- `kubectl get daemonsets`