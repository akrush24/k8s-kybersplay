=== Installation ===
```
# git clone https://github.com/kubernetes-sigs/kubespray.git
# cd ./kubespray && git checkout v2.12.1 && cd ..
# sudo pip install -r kubespray/requirements.txt
# cp -rfp ./kubespray/inventory/sample mycluster
# diff -r mycluster/group_vars/k8s-cluster/addons.yml kubespray/inventory/sample/group_vars/k8s-cluster/addons.yml
< helm_enabled: true
---
> helm_enabled: false
< kube_network_plugin: flannel
---
> kube_network_plugin: calico

# ansible-playbook -i ./mycluster/inventory.ini kubespray/cluster.yml
```
in master nodes:
```
yum install bash-completion
echo 'source <(kubectl completion bash)' >>~/.bashrc
kubectl completion bash >/etc/bash_completion.d/kubectl
```

For Ingress add single IP for each master nodes and run
```
# kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.35.0/deploy/static/provider/baremetal/deploy.yaml
# kubectl apply -f svc-ingress-nginx.yml

```
test manifest with ingress
```
# kubectl  apply -f test-manifest.yml
```

fix:
```
[root@m01k8s ~]# kubectl  get nodes
NAME               STATUS   ROLES    AGE   VERSION
m01k8s.srv.local   Ready    master   12m   v1.16.7
m02k8s.srv.local   Ready    master   11m   v1.16.7
m03k8s.srv.local   Ready    master   11m   v1.16.7
n01k8s.srv.local   Ready    <none>   10m   v1.16.7
n02k8s.srv.local   Ready    <none>   10m   v1.16.7
n03k8s.srv.local   Ready    <none>   10m   v1.16.7

[root@m01k8s ~]# kubectl label nodes n01k8s.srv.local node-role.kubernetes.io/node=
node/n01k8s.srv.local labeled
[root@m01k8s ~]# kubectl label nodes n02k8s.srv.local node-role.kubernetes.io/node=
node/n02k8s.srv.local labeled
[root@m01k8s ~]# kubectl label nodes n03k8s.srv.local node-role.kubernetes.io/node=
node/n03k8s.srv.local labeled

[root@m01k8s ~]# kubectl  get nodes
NAME               STATUS   ROLES    AGE   VERSION
m01k8s.srv.local   Ready    master   13m   v1.16.7
m02k8s.srv.local   Ready    master   12m   v1.16.7
m03k8s.srv.local   Ready    master   12m   v1.16.7
n01k8s.srv.local   Ready    node     11m   v1.16.7
n02k8s.srv.local   Ready    node     11m   v1.16.7
n03k8s.srv.local   Ready    node     10m   v1.16.7
```
