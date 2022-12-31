# Plain Key/Value:
## Documentation:
- https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/
## Commands:
- `tbd`
## Usage:
- Imperative Way:
  - `kubectl run nginx --image=nginx --env"ENVIRONMENT=prod"`
- Delcarative Way:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: envar-demo
  labels:
    purpose: demonstrate-envars
spec:
  containers:
  - name: envar-demo-container
    image: gcr.io/google-samples/node-hello:1.0
    env: # THIS WAY DOESN'T SCALE VERY WELL
    - name: DEMO_GREETING
      value: "Hello from the environment"
    - name: DEMO_FAREWELL
      value: "Such a sweet sorrow"
```

# ConfigMap:
## Documentation:
- https://kubernetes.io/docs/concepts/configuration/configmap/
## Creation:
- Imperative Way:
  - `kubectl create configmap <config-name> --from-literal=<key>=<value>`
  - `kubectl create configmap app-config --from-literal=APP_COLOR=blue --from-literal=APP_MODE=prod`
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
  - `kubectl describe configmaps`

## Usage:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
  labels: simple-webapp-color
spec:
  containers:
    - name: simple-webapp-color
      image: simple-webapp-color
  envFrom:
    - configMapRef:
      name: app-config
```
---

# Secrets: