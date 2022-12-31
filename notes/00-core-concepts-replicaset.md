## ReplicaSet Example:
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        tier: front-end # THIS MUST MATCH SO THE RS CAN MANAGE THE PODS
    spec:
      containers:
      - name: nginx-container
        image: nginx
  replicas: 3
  selector:
    matchLabels:
      tier: front-end   # THIS MUST MATCH SO THE RS CAN MANAGE THE PODS
```

## ReplicaSets:
- `kubectl explain replicaset`
- `kubectl describe ReplicaSets` | `kubect describe rs`
- `kubectl apply -f replicaset-definition.yml`
- `kubectl get replicaset`
- `kubectl delete rs replicaset-1 replicaset2`
- `kubectl get rs new-replica-set -o yaml > new-replica-set.yml`
  - `kubectl replace -f new-replica-set.yml`
  - `kubectl delete pod pod1 pod2 pod3`
- `kubectl edit rs new-replica-set`
  - Scale pods up or down, then save
- `kubectl scale -replicas=2 rs new-replia-set`