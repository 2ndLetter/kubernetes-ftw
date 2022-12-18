# Manifest Files

## Example:
```yaml
apiVersion: v1 | apps/v1
kind: Pod | Service | ReplicaSet | Deployment
metadata:
  name: MyObject
spec:
```

## apiVersion: (string)
- Specifies the API version to use

## kind: (string)
- Specifies the type of resource you want to create

## metadata: (dict)
- Specifies data about the object (ie. Name, Labels, etc.)

## spec:
- Specification of the desired behavior of the object

