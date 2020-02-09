  185  echo $DEVSHELL_PROJECT_ID
  186  gcloud config list core/project
  187  DEVSHELL_PROJECT_ID=eminent-maker-258601
  188  echo $DEVSHELL_PROJECT_ID
  189  gcloud projects get-iam-policy $DEVSHELL_PROJECT_ID --format=json >./policy.json
  190  ls
  191  nano policy.json
  192  gcloud projects set-iam-policy $DEVSHELL_PROJECT_ID ./policy.json
  193  gsutil mb gs://temt_llc/audit-log
  194  gsutil mb gs://temt_llc/audit_log
  195  gsutil mb gs://temt_llc_audit_log
  196  ls
  197  gcloud cp policy.json gs://temt_llc_audit_log
  198  gsutil cp policy.json gs://temt_llc_audit_log
  199  gcloud compute networks create temt_network --subnet-mode=auto
  200  gcloud compute networks create temt-network --subnet-mode=auto
  201  gcloud compute networks --help
  202  gcloud compute networks delete temt-network
  203  gcloud compute instaces create default-us-vm --zone=us-west1-a --network=temtprivatesubnet
  204  gcloud compute instances create default-us-vm --zone=us-west1-a --network=temtprivatesubnet
  205  gcloud compute instances create default-us-vm --zone=us-west1-a --network=temtprivatenet2
  206  gcloud compute instances create default-us-vm --zone=us-west1-a --network=temtprivatenet2 --subnet-mode=custom
  207  gcloud compute instances create default-us-vm --zone=us-west1-a --network=temtprivatenet2 --subnet=custom
  208  gcloud compute instances create default-us-vm --zone=us-west1-a --network=temtprivatenet2 --subnet=temtprivatesubnet
  209  gcloud compute instances --help
  210  gcloud compute instances delete default-us-vm
  211  echo $DEVSHELL_PROJECT_ID
  212  gcloud logging read "logName=projects/eminent-maker-258601/logs/cloudaudit.googleapis.com%2Factivity  \
  213  AND protoPayload.serviceName=storage.googleapis.com"
