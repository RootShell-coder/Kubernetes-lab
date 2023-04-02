# Kubernetes 3 master, 3 worker labs

## KVM, debian 11, CRI containerd

| `hostname`     | `k8s0.cluster.loc` | `k8s1.cluster.loc` | `k8s2.cluster.loc` | `k8s3.cluster.loc` | `k8s4.cluster.loc` | `k8s5.cluster.loc` |
| -------------- | ------------------ | ------------------ | ------------------ | ------------------ | ------------------ | ------------------ |
| `short name`   | _k8s0_             | _k8s1_             | _k8s2_             | _k8s3_             | _k8s4_             | _k8s5_             |
| `public net`   | _192.168.10.113_   | _192.168.10.114_   | _192.168.10.115_   | _192.168.10.116_   | _192.168.10.117_   | _192.168.10.118_   |
| `link k8s net` | _192.168.100.2_    | _192.168.100.3_    | _192.168.100.4_    | _192.168.100.5_    | _192.168.100.6_    | _192.168.100.7_    |
| `type`         | _master_           | _master_           | _master_           | _node_             | _node_             | _node_             |

## init

```bash
kubeadm init \
--control-plane-endpoint=k8s0 \
--upload-certs
```

## join master

```bash
  kubeadm join k8s0:6443 --token oadolq.lztrmgeabo3fkgw6 \
	--discovery-token-ca-cert-hash sha256:eec4637d5064a8cef167acfa6e058cc32e186e3baac8fc29eda866969c264513 \
	--control-plane --certificate-key 0de959a8c031feb76e4bc770152d6c68948fa610c64c248b7157346c88325d66

```

## join worker

```bash
kubeadm join k8s0:6443 --token oadolq.lztrmgeabo3fkgw6 \
	--discovery-token-ca-cert-hash sha256:eec4637d5064a8cef167acfa6e058cc32e186e3baac8fc29eda866969c264513
```

## copy admin.conf

```bash
mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

```

## get node

```bash
kubectl get node
NAME   STATUS     ROLES           AGE     VERSION
k8s0   NotReady   control-plane   4m17s   v1.26.3
k8s1   NotReady   control-plane   3m26s   v1.26.3
k8s2   NotReady   control-plane   3m13s   v1.26.3
k8s3   NotReady   <none>          2m36s   v1.26.3
k8s4   NotReady   <none>          2m37s   v1.26.3
k8s5   NotReady   <none>          2m42s   v1.26.3

```

## calico

```bash
curl https://docs.tigera.io/archive/v3.25/manifests/calico.yaml -O
kubectl apply -f calico.yaml
```

## get all pods

```bash
kubectl get pods -A
NAMESPACE     NAME                                      READY   STATUS     RESTARTS      AGE
kube-system   calico-kube-controllers-57b57c56f-d6spj   0/1     Pending    0             43s
kube-system   calico-node-9qqwg                         0/1     Init:0/3   0             43s
kube-system   calico-node-pdfbp                         0/1     Init:0/3   0             42s
kube-system   calico-node-qtjr8                         0/1     Init:0/3   0             42s
kube-system   calico-node-v9m2k                         0/1     Init:0/3   0             43s
kube-system   calico-node-zf742                         0/1     Init:0/3   0             43s
kube-system   calico-node-zx4t6                         0/1     Init:0/3   0             42s
kube-system   coredns-787d4945fb-qzxbb                  0/1     Pending    0             14m
kube-system   coredns-787d4945fb-sh8k4                  0/1     Pending    0             14m
kube-system   etcd-k8s0                                 1/1     Running    0             14m
kube-system   etcd-k8s1                                 1/1     Running    0             13m
kube-system   etcd-k8s2                                 1/1     Running    0             13m
kube-system   kube-apiserver-k8s0                       1/1     Running    0             14m
kube-system   kube-apiserver-k8s1                       1/1     Running    0             13m
kube-system   kube-apiserver-k8s2                       1/1     Running    0             13m
kube-system   kube-controller-manager-k8s0              1/1     Running    1 (13m ago)   14m
kube-system   kube-controller-manager-k8s1              1/1     Running    0             13m
kube-system   kube-controller-manager-k8s2              1/1     Running    0             13m
kube-system   kube-proxy-9v6qv                          1/1     Running    0             13m
kube-system   kube-proxy-f7pk8                          1/1     Running    0             14m
kube-system   kube-proxy-frp7j                          1/1     Running    0             13m
kube-system   kube-proxy-jn4b5                          1/1     Running    0             13m
kube-system   kube-proxy-jnm6x                          1/1     Running    0             13m
kube-system   kube-proxy-mhppb                          1/1     Running    0             14m
kube-system   kube-scheduler-k8s0                       1/1     Running    1 (13m ago)   14m
kube-system   kube-scheduler-k8s1                       1/1     Running    0             13m
kube-system   kube-scheduler-k8s2                       1/1     Running    0             13m
```

_waiting 2 minutes_

## get node Status Ready

```
kubectl get nodes
NAME   STATUS   ROLES           AGE   VERSION
k8s0   Ready    control-plane   17m   v1.26.3
k8s1   Ready    control-plane   16m   v1.26.3
k8s2   Ready    control-plane   15m   v1.26.3
k8s3   Ready    <none>          15m   v1.26.3
k8s4   Ready    <none>          15m   v1.26.3
k8s5   Ready    <none>          15m   v1.26.3
```

---

`crictl --runtime-endpoint unix:///var/run/containerd/containerd.sock ps`

```bash
crictl --runtime-endpoint unix:///var/run/containerd/containerd.sock ps
CONTAINER           IMAGE               CREATED             STATE               NAME                      ATTEMPT             POD ID              POD
978fd48065185       5a79047369329       About an hour ago   Running             kube-scheduler            2                   374da9a14d159       kube-scheduler-k8s0
8aae4267527ea       08616d26b8e74       About an hour ago   Running             calico-node               0                   4e038b3d83379       calico-node-zx4t6
00598f570aaa3       ce8c2293ef09c       About an hour ago   Running             kube-controller-manager   1                   bb2ba2ce80921       kube-controller-manager-k8s0
b37170f8170c6       92ed2bec97a63       About an hour ago   Running             kube-proxy                0                   127b8fc58596c       kube-proxy-f7pk8
b1452f41615bc       fce326961ae2d       About an hour ago   Running             etcd                      0                   efa938b47f460       etcd-k8s0
b078c6c49269f       1d9b3cbae03ce       About an hour ago   Running             kube-apiserver            0                   be748fcd95503       kube-apiserver-k8s0

```
