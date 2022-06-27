# Commands relating to networking in GCP

## VPC Networks

- Create a VPC network with a custom subnet model  
  `gcloud compute networks create <network-name> --subnet-mode=custom`

- Creates a subnet in the specified network with the specified range  
  `gcloud compute networks subnets create <subnet-name> --network=<network-name> --region=us-central1 -range=172.16.0.0/24`

- list available VPC networks  
  `gcloud compute networks list`

- list subnets (sorted by network)  
  `gcloud compute networks subnets list --sort-by=NETWORK`

## Firewall rules

These are rules that determine ingress and egress for a VPC.

- Create a firewall rule to allow ingress on port 80 on vms having the specified tag  
  `gcloud compute firewall-rules create <rule-name> --target-tags <tag> --allow tcp:80`

- Create a firewall rule to allow access from google health check addresses  
   `gcloud compute firewall-rules create <rule-name> --network=default --action=allow --direction=ingress \ --source-ranges=130.211.0.0/22,35.191.0.0/16 \ --target-tags=allow-health-check \ --rules=tcp:80 `

- General rule to allow common types of traffic to instances in a vpc network  
  `gcloud compute firewall-rules create privatenet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=privatenet --action=ALLOW --rules=icmp,tcp:22,tcp:3389 --source-ranges=0.0.0.0/0`

- list all firewall rules (sorted by network)
  `gcloud compute firewall-rules list --sort-by=NETWORK`
