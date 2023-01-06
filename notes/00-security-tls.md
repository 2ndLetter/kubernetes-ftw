# TLS:
## Documentation:
- https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/

## Notes:

### Server Certificates:
- KUBE-API SERVER: #
  - apiserver.crt (cert)
  - apiserver.key (priv key)
  - apiserver-etcd-client.crt    # <---- ACCESSES THE ETCD SERVER
  - apiserver-etcd-client.key    # <---- ACCESSES THE ETCD SERVER
  - apiserver-kubelet-client.crt # <---- ACCESSES THE KUBELET SERVER
  - apiserver-kubelet-client.key # <---- ACCESSES THE KUBELET SERVER
- ETCD SERVER:
  - etcdserver.crt (cert)
  - etcdserver.key (priv key)
- KUBELET SERVER:
  - kubelet.crt (cert)
  - kubelet.key (priv key)
### Client Certificates:
- ADMIN: (kubectl REST API) # <--------- ACCESSES THE KUBE-API SERVER
  - admin.crt (cert)
  - admin.key (priv key)
- KUBE-SCHEDULER:           # <--------- ACCESSES THE KUBE-API SERVER
  - scheduler.crt (cert)
  - scheduler.key (priv key)
- KUBE-CONTROLLER-MANAGER:  # <--------- ACCESSES THE KUBE-API SERVER
  - controller-manager.crt (cert)
  - controller-manager.key (priv key)
- KUBE-PROXY:               # <--------- ACCESSES THE KUBE-API SERVER
  - kube-proxy.crt (cert)
  - kybe-proxy.key (priv key)
