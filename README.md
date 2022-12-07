# kubernetes-ftw

*The purpose of this **unfinished** repository is to document my Kubernetes journey, and nerd-out!*

## To Do:

## Learning Material:
- [x] [Docker Certified Associate (A Cloud Guru)](https://learn.acloud.guru/course/6b00566d-6246-4ebe-8257-f98f989321cf/overview)
- [x] [Learn Kubernetes by Doing (A Cloud Guru)](https://learn.acloud.guru/course/82b39fac-b9f7-43d1-8f52-6a89efe5202f/dashboard)
- [ ] [Certified Kubernetes Administrator (CKA) with Practice Tests (Udemy)](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/)

## Lessons Learned:
- I completed the ACG Docker course to prepare me for diving into Kubernetes
- The Udemy course was hard to follow when I had zero hands-on experience
- After a few videos in, I pivoted to the ACG Learn Kubernetes by Doing course to get my hands dirty
- This was a great move!
- The Udemy course is a GREAT resource


## Useful commands:
- `kubectl --help` [Help documentation]
- `kubectl get pods` [List pods in default namespace, add `-o wide` for more details]
- `kubectl run <pod_name> --image=<image_name> --dry-run=client -o yaml > file.yaml` [Create pod-definition]
- `kubectl run nginx --image=nginx` [Create nginx pod]
- `kubectl describe pod <pod_name>` [Get specific pod details]
- `kubectl delete pod <pod_name>` [Delete pod]
- `kubectl create -f FILENAME` [Create a resource from a file or from stdin]
- `kubectl apply -f FILENAME` [Apply a config to a resource by file name or stdin]
- `kubectl scale ...` [Set a new size for a Deployment, RelicaSet, Replication Controller, or StatefulSet]
- `kubectl replace ...` [Replace a resource by filename or stdin]

## Kubernetes Architecture: [LINK](https://kubernetes.io/docs/concepts/overview/components/)
- Control Plane:
    - ETCD: Highly-available key-value store used to store all cluster data
    - kube-apiserver: Exposes the Kubernetes API
    - kube Controller Manager: Runs controller processes "Node, Job, EndpointSlice, ServiceAccount"
    - kube-scheduler: Chooses which node to run a new Pod if its not already assigned
- Worker Node:
    - kubelet: An agent that runs on each node, ensuring the containers are running in a Pod
    - kube-proxy: Network proxy that runs on each node
    - Container Runtime Engine: Software responsible for running containers
- High Level architecture:
![alt text](kubernetes__architecture.PNG "High Level")
- Low Level architecture:
![alt text](pod_node_container.PNG "Low Level")