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

- Check the recommendation hub frequently

How to determine cpu usage for horizontal pod autoscaling
formula = 1-buffer/1+traffic
1 - 15%buffer / 1 + traffic growth
buffer - percentage of free utilization at any given moment
traffic growth - max rate of increase in traffic over a 2/3 minute period.

use pause pods

### Create an infinite number of queries for load-testing

`kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://<url-to-be-tested>; done"`

## Steps to test your application

- Run your application as a single pod replica and test it under load (no autoscaling)
- use the info to configure resource requests and limits; try to find a golden ratio of ram to cpu utilization
- Test the app again with cluster autoscaling and horizontal/vertical autoscaling
- Then test large rapid spikes in demand to see how your app responds
- Try to use the same region/zone as where your app is running
- set memory limit equal to memory request
- set cpu limit higher than cpu request

## How to minimize startup time of a pod/node

- minimize image size
- minimize base image size
- minimize time between startup and ready (loading caches and classes ca significantly increase time)

## Ensure your app can shut down gracefully

- Finish active/new requests on sigterm
- update readiness probe, clean up
- use preStop hook for graceful shutdown of pods
- Configure terminationGracePeriodsSeconds

## Readiness and liveness probes

- Always define readiness probes
- Ready = true when truly ready
- make sure the code for probe is simple and isolated

# Handle retries

- use exponential backoff to protect against overwhelming the service

# Batch application optimization

- use dedicated node pools
- use optimize-utilization
- separate applications with node auto provisioning
- use pre-emptible vms if your apps are fault tolerant

# Serving application optimization

- ensure that workloads are lean and optimized with quick startup times
- if you have a service that uses resources with a long provisioning time (like gpus) then it is best to over-provision, maybe using a pause pod
- use NodeLocal DNS Caching
