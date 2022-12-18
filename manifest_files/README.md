# Manifest Files

## Example:
```yaml
apiVersion: v1 | apps/v1
kind: Pod | Service | ReplicaSet | Deployment
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
```

## apiVersion: (string)
- Specifies the API version to use

## kind: (string)
- Specifies the type of resource you want to create

## metadata: (dictionary)
- Specifies data about the object (ie. Name, Labels, etc.)

## spec: (object)
- Specification of the desired behavior of the object

