# manifest_files

## apiVersion: (string)
- Specifies the API version to use

## kind: (string)
- Specifies the type of resource you want to create

## metadata: (dict)
- Specifies data about the object (ie. Name, Labels, etc.)

## spec:


```yaml
apiVersion: v1 | apps/v1
kind: Pod | Service | ReplicaSet | Deployment
metadata:
  name: nginx
spec:
```