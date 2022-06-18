### Enable cluster autoscaling

`gcloud beta container clusters update scaling-demo --enable-autoscaling --min-nodes 1 --max-nodes 5`

### Switch to optimize utilization autoscaling

`gcloud beta container clusters update scaling-demo --autoscaling-profile optimize-utilization`

### describe a gcloud cluster

`gcloud container clusters describe scaling-demo `
