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