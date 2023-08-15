# Article

https://milindchawre.github.io/site/blog/k3s-upgrade-a-simplified-guide/

# Install System Upgrade Controller

```
kubectl apply -f https://raw.githubusercontent.com/rancher/system-upgrade-controller/master/manifests/system-upgrade-controller.yaml

kubectl get all -n system-upgrade
```

# Label Nodes

```
kubectl label node rpi-1 k3s-master-upgrade=true
kubectl label node rpi-2 k3s-worker-upgrade=true
kubectl label node rpi-3 k3s-worker-upgrade=true
```

# Perform Upgrade

Edit `plans.yaml` to specify the version to upgrade to.

```
kubectl apply -f upgrade/plans.yaml
kubectl get nodes
```

# Upgrade Longhorn

Upgrade Longhor Manager:
```
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.5.1/deploy/longhorn.yaml

```

Upgrade Longhorn Engine from UI:
- Go to each volume and choose Upgrade Engine Image
- Go to Settings -> Engine Image
- Delete old image when no more references
