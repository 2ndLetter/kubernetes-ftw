## ENV Variables - PODs:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
spec:
  containers:
    - name: simple-webapp-color
      image: simple-webapp-color
  env:                # PLAIN KEY VALUE
    - name: APP_COLOR
      value: pink
```
```yaml
env:                  # CONFIGMAP
  - name: APP_COLOR
    valueFrom:
      configMapKeyRef:
```
```yaml
env:                  # SECRETS
  - name: APP_COLOR
    valueFrom:
      secretKeyRef:
```
- `docker run -e APP_COLOR=pink simple-webapp-color` # DOCKER RUN WAY