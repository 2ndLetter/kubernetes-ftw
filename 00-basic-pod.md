# basic-pod:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp      # CAN BE USED TO LINK POD TO SERVICE
    tier: front-end # CAN BE USED TO LINK POD TO SERVICE
  namespace: <namespace_name>
spec:
  containers:
  - name: nginx-container
    image: nginx
```