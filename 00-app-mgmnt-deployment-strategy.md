## Deployment Update Strategy Example:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        tier: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx
  replicas: 3
  selector:
    matchLabels:
      tier: front-end
  strategy:
    type: RollingUpdate | Recreate
    rollingUpdate:        # USE IF TYPE IS RollingUpdate
      maxSurge: 25%       # USE IF TYPE IS RollingUpdate
      maxUnavailable: 25% # USE IF TYPE IS RollingUpdate
```