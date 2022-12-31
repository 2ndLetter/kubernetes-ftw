## Resource Requests - PODs: [documentation](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#example-1)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-web-color
  labels:
    name: simple-web-color
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
      - containerPort: 8080
    resources:
      requests:
        memory: "1Gi"
        cpu: 1
```