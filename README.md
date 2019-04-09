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

Still working on the slurm setup ...

