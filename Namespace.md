ubuntu@ip-172-31-47-69:~/minikube$ kubectl get ns
NAME              STATUS   AGE
default           Active   3m37s
kube-node-lease   Active   3m37s
kube-public       Active   3m37s
kube-system       Active   3m37s

ubuntu@ip-172-31-47-69:~/minikube$ kubectl get pods -n kube-system
NAME                               READY   STATUS    RESTARTS       AGE
coredns-66bc5c9577-l2fgg           1/1     Running   0              4m32s
etcd-minikube                      1/1     Running   0              4m37s
kube-apiserver-minikube            1/1     Running   0              4m38s
kube-controller-manager-minikube   1/1     Running   0              4m37s
kube-proxy-t4k74                   1/1     Running   0              4m33s
kube-scheduler-minikube            1/1     Running   0              4m37s
storage-provisioner                1/1     Running   1 (4m2s ago)   4m35s

