# Optimization steps for GKE Cluster

## For Dev (non-prod) environments!

- Disable horizontal pod autoscaling
  `gcloud container clusters update <cluster-name> --update-addons=HorizontalPodAutoscaling=DISABLED`

- Disable kube-dns
  `kubectl scale --replicas=0 deployment/kube-dns-autoscaler --namespace=kube-system`

- Limit kube-dns scaling
  `kubectl scale --replicas=0 deployment/kube-dns-autoscaler --namespace=kube-system`
  `kubectl scale --replicas=1 deployment/kube-dns --namespace=kube-system`

- Use resource quota, example can be found at ~/kubernetes/resourcequota.yaml

- Use limit ranges, example file at ~/kubernetes/limitrange.yaml
- Add scale down delay to metric servers to prevent too many restarts
- Use a tool like kpt to validate changes
- Pre-emtible vms
- Run your application as a single pod replica and test it under load
- Check the recommendation hub frequently

How to determine cpu usage for horizontal pod autoscaling
formula = 1-buffer/1+traffic
1 - 15%buffer / 1 + traffic growth
buffer - percentage of free utilization at any given moment
traffic growth - max rate of increase in traffic over a 2/3 minute period.

use pause pods

### Create an infinite number of queries for load-testing

`kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://<url-to-be-tested>; done"`
