# Commands relating to GCP IAM

## Service Accounts

- Create service account  
  `gcloud iam service-accounts create <name> --display-name "<display-name>"`

- Create keys for a service account  
  `gcloud iam service-accounts keys create /tmp/key.json --iam-account team-a-dev@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com`

- Add role binding to service account  
  `gcloud projects add-iam-policy-binding $GOOGLE_CLOUD_PROJECT --member serviceAccount:<account-name>@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com --role roles/viewer`
