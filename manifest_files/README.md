# Manifest Files

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

## apiVersion: (string)
- Specifies the API version to use

## kind: (string)
- Specifies the type of resource you want to create

## metadata: (dict)
- Specifies data about the object (ie. Name, Labels, etc.)

## spec: (dict)
- Specification of the desired behavior of the object

## Pod Manifest Example:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
    tier: frontend
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```