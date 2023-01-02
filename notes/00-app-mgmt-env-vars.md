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
- https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/
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

## KodeKloud Lab:
- `kubectl get configmaps`
- `kubectl get po webapp-color -o yaml > webapp-color.yaml`
- Change plain key/value via VIM
- `kubectl replace --force -f webapp-color.yaml`
- `kubectl create configmap webapp-config-map --from-literal=APP_COLOR=darkblue --dry-run=client -o yaml > webapp-config-map.yaml`
```yaml
apiVersion: v1
data:
  APP_COLOR: darkblue
kind: ConfigMap
metadata:
  creationTimestamp: null
  name: webapp-config-map
```
- `kubectl apply -f webapp-config-map.yaml`
- Change webapp-color.yaml to use configmap via VIM:
```yaml
# Snippet
spec:
  containers:
    - envFrom:
      - configMapRef:
          name: webapp-config-map
```
- `kubectl replace --force -f webapp-color.yaml`
---

# Secrets:
## Documenation:
- https://kubernetes.io/docs/concepts/configuration/secret/

## Creation:
- Imperative Way:
  - `kubectl create secret generic <secret-name> --from-literal=<key>=<value> --from-literal=<key>=<value>`
  - `kubectl create secret generic <secret-name> --from-file=<path-to-file>`
    - Example: `kubectl create secret generic app-secret --from-file=app_secret.properties`
- Declarative Way:
  - secret-data.yaml:
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_Host: mysql # DB_HOST: bXlzcWw=
  DB_User: root
  DB_Password: paswrd
```
  - `kubectl create -f secret-data.yaml`
  - base64 encoding:
    - `echo -n '<value>' | base64`
    - Example:
      - `echo -n 'mysql' | base64`
        ```
        bXlzcWw=
        ```
  - base64 decoding:
    - `echo -n <value> | base64 --decode`
    - Example:
      - `echo -n 'bXlzcWw=' | base64`
        ```
        mysql
        ```

## Commands:
- `kubectl get secrets`
- `kubectl describe secrets` # HIDES VALUES
- `kubectl get secrets <secret-name> -o yaml` # SHOWS VALUES

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
  - secretRef:
      name: app-secret
```
- `kubectl created -f pod-definition.yaml`

## NEED TO MAKE NOTES ABOUT "SECRETS IN PODS" WHICH SHOWS VOLUMES

## KodeKloud Lab:
- `kubectl ...`
