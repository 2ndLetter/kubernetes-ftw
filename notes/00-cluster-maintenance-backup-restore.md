# Backup and Restore Methods:
## Documentation:
- https://kubernetes.io/docs/home/

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
