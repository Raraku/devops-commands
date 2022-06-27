# Useful commands for interacting via the gcloud / cloud console

- List active accounts

  `gcloud auth list`

- list projects currently associated

  `gcloud config list project`

- Switch the terminal to the indicated service account

  `gcloud auth activate-service-account --key-file=<file-location>`

- Get the default zone  
  `gcloud config get-value compute/zone`

  switch zone for region to get the default region

- set compute zone/region  
  `gcloud config set compute/zone us-central1-a`

- Retrieve info about a project  
  `gcloud compute project-info describe -project <project-id>`

- list all configurations in the environment  
  `gcloud config list`

- switch to specified account  
  `gcloud config set account <username>`
