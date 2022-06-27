- Enable cluster autoscaling  
  `gcloud beta container clusters update scaling-demo --enable-autoscaling --min-nodes 1 --max-nodes 5`

- Switch to optimize utilization autoscaling  
  `gcloud beta container clusters update scaling-demo --autoscaling-profile optimize-utilization`

- describe a gcloud cluster  
  `gcloud container clusters describe scaling-demo `

- Container - native load balancing  
   Create a cluster with the following command
  `gcloud container clusters create tes --num-nodes=3 --enable-ip-alias`
  The --enable-ip-alias flag is included in order to enable the use of alias IPs for pods which will be required for container-native load balancing through an ingress.

- Create a cluster-ip for neg
  The cluster-ip will be used to route traffic to your application pod to allow GKE to create a network endpoint group

```
apiVersion: v1
kind: Service
metadata:
  name: gb-frontend-svc
  annotations:
    cloud.google.com/neg: '{"ingress": true}'
spec:
  type: ClusterIP
  selector:
    app: gb-frontend
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
```

- Create Ingress

````cat << EOF > gb_frontend_ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: gb-frontend-ingress
spec:
  defaultBackend:
    service:
      name: gb-frontend-svc
      port:
        number: 80
EOF```
````

- Resize containers in a cluster  
  `gcloud container clusters resize <cluster-name> --zone <zone> --num-nodes=4`
