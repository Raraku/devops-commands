# Commands related to gsutil and cloud storage

- Create bucket  
  `gsutil mb gs://<bucket-name>`

- Get acl permissions for an object  
  `gsutil acl get gs://<bucket-name>/<object-path>`

- Make an object/bucket private  
  `gsutil acl set private gs://<bucket-name>/<object-path>

- sync two directories/buckets  
  `gsutil rsync -r <source> <dest>`
  source/dest can be a directory or bucket

- grant all users the viewer permissions on a resource  
  `gsutil iam ch allUsers:objectViewer <bucket-path>`
