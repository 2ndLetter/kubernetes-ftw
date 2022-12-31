# ConfigMap

## Documentation:
- https://kubernetes.io/docs/concepts/configuration/configmap/

## Creation:
- Imperative Way:
  - `kubectl create configmap <config-name> --from-literal=<key>=<value>`
  - `kubectl create configmap app-config --from-literal=APP_COLOR=blue --from-literal=APP_MOD=prod`
  - `kubectl create configmap app-config --from-file=<path-to-file>`
- Declarative Way:
  - config-map.yaml:
  ```yaml
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: app-config
  data:
    APP_COLOR: blue
    APP_MODE: prod
  ```
  - `kubectl create -f config-map.yaml`

## Commands:
  - `kubectl get configmaps`

## Usage:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
spec:
  containers:
    - name: simple-webapp-color
      image: simple-webapp-color
  env:
    - name: APP_COLOR
      value: pink
```
```yaml
# CONFIGMAP
env:
  - name: APP_COLOR
    valueFrom:
      configMapKeyRef:
```
```yaml
# SECRETS
env:
  - name: APP_COLOR
    valueFrom:
      secretKeyRef:
```
- `docker run -e APP_COLOR=pink simple-webapp-color` # DOCKER RUN WAY

