## Labels | Selectors | Annotations:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp
  labels:
    app: App1           # LABEL
    function: Front-end # LABEL
```
- `kubectl get pods --selector app-App1` # Returns object based on label

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metdata:
  name: simple-webapp
  labels:                   # REPLICASET OBJECT LABEL
    app: App1               # REPLICASET OBJECT LABEL
    function: Front-end     # REPLICASET OBJECT LABEL
  annotations:
    buildversion: 1.34      # METADATA
spec:
  replicas: 3
  selector:                 # CONNECTS REPLICASET TO POD
    matchLabels:            # CONNECTS REPLICASET TO POD
      app: App1             # CONNECTS REPLICASET TO POD
  template:
    metadata:
      labels:               # CONNECTS REPLICASET TO POD
        app: App1           # CONNECTS REPLICASET TO POD
        function: Front-end
    spec:
      containers:
      - name: simple-webapp
        image: simple-webapp
```

## Node Selectors - PODs:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: data-processor
    image: data-processor
  nodeSelector:
    size: Large # LABELS ASSIGNED TO NODES
```
- `kubectl label nodes <node-name> <label-key>=<label-value>` # LABEL COMMAND EXAMPLE
- `kubectl label nodes node-1 size=Large` # RUN THIS COMMAND BEFORE DEPLOYING THE ABOVE POD