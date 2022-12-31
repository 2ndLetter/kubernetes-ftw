## Commands and Arguments:
- `docker run ubuntu sleep 5`
- `docker ps`
- `docker ps -a`
- DOCKERFILE ubuntu-sleeper:
  ```yaml
  FROM Ubuntu
  ENTRYPOINT ["sleep"] # ENTRYPOINT
  CMD ["5"]            # CMD
  ```
  - `docker run ubuntu-sleeper 10`
- pod-definition.yml:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: ubuntu-sleeper-pod
  spec:
    containers:
      - name: ubuntu-sleeper
        image: ubuntu-sleeper
        command: ["sleep2.0"] # OVERRIDES ENTRYPOINT
        args: ["10"]          # OVERRIDES CMD
  ```
- ubuntu-sleeper-2.yaml:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: ubuntu-sleeper-2
  spec:
    containers:
      - name: ubuntu
        image: ubuntu
        ###
        command: ["sleep", "5000"]
        OR
        command: ["sleep"]
        args: ["5000"]
        OR
        command:
          - "sleep"
          - "5000"
  ```
- `kubectl run webapp-green --image kodekloud/webapp-color --dry-run=client -o yaml --command -- --color=green > ubuntu-sleeper-2.yaml`
- `kubectl run nginx --image=nginx -- <arg1> <arg2> ... <argN>` # Start default command with custom args