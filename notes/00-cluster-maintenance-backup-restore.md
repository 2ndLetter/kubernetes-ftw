# Backup and Restore Methods:
## Documentation:
- https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster
- https://github.com/etcd-io/etcd

## Usage:
- Imperative Way:
  - `kubectl ...`
- Delcarative Way:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: nginx-container
    image: nginx
```
## Commands:
- `kubectl get all --all-namespaces -o yaml > all-deploy-services.yaml` # Backups the resources as yaml
- `export ETCDCTL_API=3` # Set variable
- `ETCDCTL_API=3 etcdctl snapshot save </path-to-new-snapshot/>snapshot.db`
- `ETCDCTL_API=3 etcdctl snapshot status </path-to-new-snapshot/>snapshot.db`
- Restore snapshot:
  - `service kube-apiservice stop`
  - `ETCDCTL_API=3 etcdctl snapshot restore snapshot.db --data-dir /var/lib/etcd-from-backup`
  - Configure etcd.service to use new data directory:
    - `--data-dir=/var/lib/etcd-from-backup`
  - `systemctl daemon-reload`
  - `systemctl restart etcd`
  - `service kube-apiserver start`
- For each etcdctl commands add the below options:
  ```
  ETCDCTL_API=3 etcdctl \
    snapshot save snapshot.db \
    --endpoints=https://127.0.0.1:2379 \
    --cacert=/etc/etcd/ca.crt \
    --cert=/etc/etcd/etcd-server.crt \
    --key=/etc/etcd/etcd-server.key
  ```
## Notes:
- Store manifests in GitHub (SCM)
- Backup the actual directory where etcd.service keeps its data (--data-dir=/var/lib/etcd)

## KodeKloud Lab:
- `kubectl ...`
- `kubectl ... --dry-run=client -o yaml > template.yaml`
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: nginx-container
    image: nginx
```
- `kubectl apply -f template.yaml`
