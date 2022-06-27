## Cloud source repositories

- Create cloud source repository  
  `gcloud source repos create <repo-name>`

## Cloudbuild

- Build image in local directory
  `gcloud builds submit --tag <registry-repo-url> .`

## Artifact Registry

- Create artifact repository  
  `gcloud artifacts repositories create <repo-name> --repository-format=docker --location=<region>`
