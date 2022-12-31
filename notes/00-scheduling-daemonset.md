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

## DaemonSets:
- `kubectl get daemonset -A -o wide`
- `kubectl describe ds kube-proxy -n kube-system`
- `kubectl create deplyment elasticsearch -n kube-system --image=k8s.gcr.io/fluentd-elasticsearch:1.20 --dry-run=client -o yaml > fluentd.yaml`
- Change kind to DaemonSet, remove replicas
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: elasticsearch
  namespace: kube-system
spec:
  selector:
    matchLabels:
      name: elasticsearch
  template:
    metadata:
      labels:
        name: elasticsearch
    spec:
      containers:
      - name: elasticsearch
        image: k8s.gcr.io/fluentd-elasticsearch:1.20
```
- `kubectl apply -f fluentd.yaml`