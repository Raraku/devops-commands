# Kubectl commands relating to nodes.

## Cordon off a set of nodes

```
for node in $(kubectl get nodes -l cloud.google.com/gke-nodepool=node -o=name); do
  kubectl cordon "$node";
done
```

## Forcefully Drain a node-pool (set of nodes)

```
for node in $(kubectl get nodes -l cloud.google.com/gke-nodepool=node -o=name); do
  kubectl drain --force --ignore-daemonsets --delete-local-data --grace-period=10 "$node";
done
```
