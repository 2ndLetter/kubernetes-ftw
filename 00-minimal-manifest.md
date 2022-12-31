## Minimal Example:
```yaml
apiVersion: v1 | apps/v1
kind: Pod | Service | ReplicaSet | Deployment
metadata:
  name: <name_of_object>
  labels:
    <key>: <value>
    <key>: <value>
spec:
  <object_specific_item>
    <key>: <value>
    <key>: <value>
```