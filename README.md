# Collaboration Space

__RULES__: No propriety information and/or content

## Subdirectories

### Vagrant k8s/slurm cluster -- kubernetes-cluster

#### Configuration

> 3 nodes
 k8s-head
 k8s-node-[1-2]

```bash
cd kubernetes-cluster
vagrant up
vagrant ssh k8s-head
```

```bash
vagrant@k8s-head:~$ kubectl get nodes -o wide
NAME         STATUS   ROLES    AGE     VERSION   INTERNAL-IP      EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
k8s-head     Ready    master   5m36s   v1.14.1   192.168.205.10   <none>        Ubuntu 16.04.6 LTS   4.4.0-143-generic   docker://17.3.3
k8s-node-1   Ready    <none>   3m44s   v1.14.1   192.168.205.11   <none>        Ubuntu 16.04.6 LTS   4.4.0-143-generic   docker://17.3.3
k8s-node-2   Ready    <none>   117s    v1.14.1   192.168.205.12   <none>        Ubuntu 16.04.6 LTS   4.4.0-143-generic   docker://17.3.3
```

### Argo

__Remember__ : sudo to root for admin stuff

1. Download
```bash
sudo curl -sSL -o /usr/local/bin/argo https://github.com/argoproj/argo/releases/download/v2.2.1/argo-linux-amd64
sudo chmod +x /usr/local/bin/argo
```

2. Install the Controller and UI
```bash
vagrant@k8s-head:~$ kubectl create ns argo
namespace/argo created
vagrant@k8s-head:~$ kubectl apply -n argo -f https://raw.githubusercontent.com/argoproj/argo/v2.2.1/manifests/install.yaml
customresourcedefinition.apiextensions.k8s.io/workflows.argoproj.io created
serviceaccount/argo created
serviceaccount/argo-ui created
clusterrole.rbac.authorization.k8s.io/argo-aggregate-to-admin created
clusterrole.rbac.authorization.k8s.io/argo-aggregate-to-edit created
clusterrole.rbac.authorization.k8s.io/argo-aggregate-to-view created
clusterrole.rbac.authorization.k8s.io/argo-cluster-role created
clusterrole.rbac.authorization.k8s.io/argo-ui-cluster-role created
clusterrolebinding.rbac.authorization.k8s.io/argo-binding created
clusterrolebinding.rbac.authorization.k8s.io/argo-ui-binding created
configmap/workflow-controller-configmap created
service/argo-ui created
deployment.apps/argo-ui created
deployment.apps/workflow-controller created
```

3. Configure the service account to run workflows

To run all of the examples in this guide, the 'default' service account is too limited to support features such as artifacts, outputs, access to secrets, etc... For demo purposes, run the following command to grant admin privileges to the 'default' service account in the namespace 'default':

```bash
kubectl create rolebinding default-admin --clusterrole=admin --serviceaccount=default:default
```
4. Run Simple Example Workflows

```bash
argo submit --watch https://raw.githubusercontent.com/argoproj/argo/master/examples/hello-world.yaml

 ● hello-world-cxnh8  hello-world-cxnh8  38s
Name:                hello-world-cxnh8
Namespace:           default
ServiceAccount:      default
Status:              Succeeded
Created:             Tue Apr 09 22:09:29 +0000 (39 seconds ago)
Started:             Tue Apr 09 22:09:29 +0000 (39 seconds ago)
Finished:            Tue Apr 09 22:10:08 +0000 (now)
Duration:            39 seconds

STEP                  PODNAME            DURATION  MESSAGE
 ✔ hello-world-cxnh8  hello-world-cxnh8  38s
 ```

 See what ran
 ```bash
 vagrant@k8s-head:~$ argo list
NAME                STATUS      AGE   DURATION
hello-world-cxnh8   Succeeded   1m    39s
```

Get the application status
```bash
vagrant@k8s-head:~$ argo get hello-world-cxnh8
Name:                hello-world-cxnh8
Namespace:           default
ServiceAccount:      default
Status:              Succeeded
Created:             Tue Apr 09 22:09:29 +0000 (1 minute ago)
Started:             Tue Apr 09 22:09:29 +0000 (1 minute ago)
Finished:            Tue Apr 09 22:10:08 +0000 (1 minute ago)
Duration:            39 seconds

STEP                  PODNAME            DURATION  MESSAGE
 ✔ hello-world-cxnh8  hello-world-cxnh8  38s
 ```

 Get the logs/output

 ```bash
 vagrant@k8s-head:~$ argo logs  hello-world-cxnh8
 _____________
< hello world >
 -------------
    \
     \
      \
                    ##        .
              ## ## ##       ==
           ## ## ## ##      ===
       /""""""""""""""""___/ ===
  ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
       \______ o          __/
        \    \        __/
          \____\______/
```


![dilbert](https://cdn-images-1.medium.com/max/1600/1*s73uVVGvm5RGkekQJYLwyg.png)



Still working on the slurm setup ...