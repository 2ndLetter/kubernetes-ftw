## Node Affinity - PODs: [documentation](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: data-processor
spec:
  containers:
  - name: data-processor
    image: data-processor
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In | NotIn | Exists
            values:
            - Large
            - Medium
```

## Node Affinity:
- Learn to manipulate multiple lines via VIM, improper spacing causes problems
  - Shift + V
  - Shift + >
- `kubectl label node node01 color=blue`
- `kubectl create deployment blue --image=nginx --replicas=3 --dry-run=client -o yaml > blue.yaml`
  - `kubectl apply -f blue.yaml`
- `kubectl describe node controlplane | grep Taint`
- Manifest files used:
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    creationTimestamp: null
    labels:
      app: blue
    name: blue
  spec:
    replicas: 3
    selector:
      matchLabels:
        app: blue
    strategy: {}
    template:
      metadata:
        creationTimestamp: null
        labels:
          app: blue
      spec:
        containers:
        - image: nginx
          name: nginx
          resources: {}
        affinity:
          nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
              nodeSelectorTerms:
              - matchExpressions:
                - key: color
                  operator: In
                  values:
                  - blue
  ```
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    creationTimestamp: null
    labels:
      app: red
    name: red
  spec:
    replicas: 2
    selector:
      matchLabels:
        app: red
    strategy: {}
    template:
      metadata:
        creationTimestamp: null
        labels:
          app: red
      spec:
        containers:
        - image: nginx
          name: nginx
          resources: {}
        affinity:
          nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
              nodeSelectorTerms:
              - matchExpressions:
                - key: node-role.kubernetes.io/control-plane
                  operator: Exists
  
  status: {}
  ```