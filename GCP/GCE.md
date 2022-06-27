# Commands related to Google Compute Engine

## Instances

- Create an instance  
  `gcloud compute instances create <instance-name> —machine-type n1-standard-2 —zone us-central1-f`

- SSH into an instance (zone is required)
  `gcloud compute ssh <instance-name> —zone us-central1-f `

- SSH into an instance using IAP  
  `gcloud compute ssh <vm-name> --zone <zone> --tunnel-through-iap`
  35.235.240.0/20 - iap forwarding range

- Get serial port output  
  `gcloud compute instances get-serial-port-output <instance-name>`

- Reset the password of a windows VM to allow RDP access  
  `gcloud compute reset-windows-password <instance-name> -user admin -zone us-central1-a`

- Create an instance with a startup script that installes apache and replaces the default page  
  `gcloud compute instances create <instance-name> \ --image-family debian-9 \ --image-project debian-cloud \ --zone us-central1-a \ --tags network-lb-tag \ --metadata startup-script="#! /bin/bash sudo apt-get update sudo apt-get install apache2 -y sudo service apache2 restart echo '<!doctype html><html><body><h1>haha instance</h1></body></html>' | tee /var/www/html/index.html”`

# Load Balancers

Forwarding rules, url-maps, target-pools, static-ip-addresses, backend and frontend services are all listed under this category

- Create a healh check  
  `gcloud compute http-health-checks create <check-name `

- Create a target pool  
  `gcloud compute target-pools create <pool-name> --region us-central1 --http-health-check <health-check>`

- Add instances to target pool  
  `gcloud compute target-pools add-instances <pool-name> --instances ww1,ww2,ww3`

- Create forwarding rule  
  `gcloud compute forwarding-rules create <rule-name> --region us-central1 --ports 80 --address <static-ip-address> --target-pool <pool-name>`

- Create static ip address  
  `gcloud compute addresses create <static-ip-address> --ip-version=IPV4 --global`

- Create backend service  
  `gcloud compute backend-services create <backend-service-name> --protocol=HTTP --port-name=http --health-checks=<health-check-name> -global`

- Add instance group to backend  
  `gcloud compute backend-services add-backend web-backend-service \ --instance-group=lb-backend-group \ --instance-group-zone=us-central1-a \ —global`

- Create url-maps  
  `gcloud compute url-maps create web-map-http --default-service web-backend-service`

- Create target http proxy  
  `gcloud compute target-http-proxies create http-lb-proxy --url-map web-map-http`

## Instance Templates

- Create instance template  
  ` gcloud compute instance-templates create lb-backend-template \ --region=us-central1 \ --network=default \ --subnet=default \ --tags=allow-health-check \ --image-family=debian-9 \ --image-project=debian-cloud \ --metadata=startup-script='#! /bin/bash apt-get update apt-get install apache2'`

## Instance Groups

- Create instance group
  `gcloud compute instance-groups managed create <group-name> --template=<template-name> --size=2 --zone=us-central1-a`
