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
- envFrom: Injects the entire data into a pod
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
- env: Injects as a single env var into a pod
```yaml
# SNIPPET
env: 
  - name: APP_COLOR
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: APP_COLOR
```
- volumes: Injects the entire data into a pod as files in a volume
```yaml
# SNIPPET
volumes:
- name: app-config-volume
  configMap:
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
- envFrom: Injects the whole Secret into a pod
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
- env: Injects as a single env var into a pod
```yaml
# SNIPPET
env: 
  - name: APP_COLOR
  valueFrom:
    secretKeyRef:
      name: app-secret
      key: DB_PASSWORD
```
- volumes: Injects the whole secret into a pod as files in a volume
```yaml
# SNIPPET
volumes:
- name: app-secret-volume
  secret:
    name: app-secret
```
  - Secrets in volumes (mounted on the Container) have a file for each Key (named as the key)
  - `ls /opt/app-secret-volumes`
    ```
    DB_Host DB_Password DB_User
    ```
  - The contents of each file is the Key's Value
  - `cat /opt/app-secret-volumes/DB_Password`
    ```
    paswrd
    ```

## Notes:
- Secrets are not Encrypte4d, only Encoded. Don't check into SCM!
- Secrets are not encrypted in ETCD, so use Encryption-at-rest
  - https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/

## KodeKloud Lab:
- `kubectl get secrets`
- `kubectl create secret generic db-secret --from-literal=DB_Host=sql01 --from-literal=DB_User=root --from-literal=DB_Password=password123 --dry-run=client -o yaml > db-secret.yaml`
```yaml
apiVersion: v1
data:
  DB_Host: c3FsMDE=
  DB_Password: cGFzc3dvcmQxMjM=
  DB_User: cm9vdA==
kind: Secret
metadata:
  creationTimestamp: null
  name: db-secret
```
- `kubectl apply -f db-secret.yaml`
- Update deployed pod to use db-secret (webapp-pod.yaml)
```yaml
# SNIPPPET
spec:
  containers:
    - envFrom:
      - secretRef:
          name: db-secret
```
- `kubectl replace --force -f webapp-pod.yaml`

## Encryption at Rest:
- Documentation:
  - https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/
- encrypt.yaml:
```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets
      - configmaps
      - pandas.awesome.bears.example
    providers:
      - aescbc:
          keys:
            - name: key1
              secret: <BASE 64 ENCODED SECRET>
      - identity: {}
```
- `kubectl apply -f encrypt.yaml`