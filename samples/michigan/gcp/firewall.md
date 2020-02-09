    1  gcloud compute sones list | grep us-west1
    2  gcloud compute zones list | grep us-west1
    3  gcloud config set project eminent-maker-258601
    4  gcloud compute zones list | grep us-west1
    5  gcloud config set compute/zones us-west-b
    6  gcloud config set compute/zones us-west1-b
    7  gcloud config set compute/zone us-west1-b
    8  gcloud compute instances create "tem-web-2"  --machine-type "f1-micro" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" -subnet "de
fault"
    9  gcloud compute instances create "tem-web-2"  --machine-type "f1-micro" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "d
efault"
   10  exit
   11  man apt-get
   12  sudo apt-get install mysql-server -y
   13  man apt-get
   14  man apt
   15  apt list
   16  man apt
   17  apt search mysql
   18  sudo mysql_secure_installation
   19  man service
   20  sudo sevrice mysql restart
   21  sudo service mysql restart
   22  sudo mysql_secure_installation
   23  apt search php
   24  sudo nano /etc/apt/sources.list
   25  sudo apt-get update
   26  sudo apt-get install php5-fpm php5-mysql
   27  history 40
   28  export LOCATION=US
   29  echo $LOCATION
   30  gsutil mb -l $LOCATION gs://temt_llc_test
   31  gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png
   32  gsutil cp my-excellent-blog.png gs://temt_llc_test/my-excellent-blog.png
   33  gsutil acl ch -u allUsers:R gs://temt_llc_test/my-excellent-blog.png
   34  env
   35  echo $MY=ZONE
   36  echo $MY_ZONE
   37  export MY_ZONE=us-west1-a
   38  echo $MY_ZONE
   39  gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 1
   40  gcloud config set project PROJECT_ID
   41  gcloud config set project eminent-maker-258601
   42  gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 1
   43  kubectl version
   44  kubectl run nginx --image=nginx:1.10.0
   45  kubectl expose deployment nginx --port 80 --type LoadBalancer
   46  kubectl get services
   47  kubectl scale deployment nginx --replicas 3
   48  ubectl get nodes
   49  ubectl get pods
   50  kubectl get pods
   51  kubectl get services
   52  git clone https://github.com/GoogleCloudPlatform/appengine-guestbook-python
   60  cat app.yaml
   61  ls -l
   62  dev_appserver.py ./app.yaml
   63  ls
   64  ls -l
   65  cat index.yaml
   66  gcloud app deploy ./index.yaml ./app.yaml
   67  gcloud app logs tail -s default
   68  vi index.yaml
   69  gcloud app deploy ./index.yaml ./app.yaml
   70  echo $MY_ZONE
   71  env
   72  export MY_ZONE=us-west1-a
   73  echo $MY_ZONE
   74  gsutil cp gs://cloud-training/gcpfcoreinfra/mydeploy.yaml mydeploy.yaml
   75  ls
   76  sed -i -e 's/PROJECT_ID/'$DEVSHELL_PROJECT_ID/ mydeploy.yaml
   77  sed -i -e 's/ZONE/'$MY_ZONE/ mydeploy.yaml
   78  nano mydeploy.yaml
   79  cat mydeploy.yaml
   80  gcloud deployment-manager deployments create my-first-dep1 --config mydeploy.yaml
   81  nano mydeploy.yaml
   82  gcloud deployment-manager deployments update my-first-dep1 --config mydeploy.yaml
   83  bg query "select string_field_10 as request, count(*) as requestcount from Load_Access_Log_Data.accesslog froup by request order by requestcount desc"
   84  bq query "select string_field_10 as request, count(*) as requestcount from Load_Access_Log_Data.accesslog froup by request order by requestcount desc"
   85  bq query "select string_field_10 as request, count(*) as requestcount from Load_Access_Log_Data.accesslog group by request order by requestcount desc"
   86  pwd
   87  nano role.yaml
   88  gcloud iam roles create app_viewer --project $DEVSHELL_PROJECT_ID --file role.yaml
   89  nano role.yaml
   90  gcloud iam roles create app_viewer --project $DEVSHELL_PROJECT_ID --file role.yaml
   91  gcloud iam roles list --project $DEVSHELL_PROJECT_ID
   92  gcloud roles describe app_viewer --project $DEVSHELL_PROJECT_ID
   93  gcloud iam roles describe app_viewer --project $DEVSHELL_PROJECT_ID
   94  nano update-role.yaml
   95  gcloud iam roles update app_viewer --project $DEVSHELL_PROJECT_ID --file update-role.yaml
   96  gcloud iam roles update app_viewer --project $DEVSHELL_PROJECT_ID --stage DISABLED
   97  gcloud iam roles delete app_viewer--project $DEVSHELL_PROJECT_ID
   98  gcloud iam roles delete app_viewer --project $DEVSHELL_PROJECT_ID
   99  gcloud iam roles list --project $DEVSHELL_PROJECT_ID --show-deleted
  100  gcloud iam roles undelete app_viewer --project $DEVSHELL_PROJECT_ID
  101  gcloud iam roles list --project $DEVSHELL_PROJECT_ID
  102  gcloud iam roles delete app_viewer --project $DEVSHELL_PROJECT_ID
  103  gcloud auth list
  104  gcloud config list project
  105  gcloud compute networks create temtnetwork --subnet-mode=auto
  106  clear
  107  gcloud compute networks create temtprivatenet
  108  gcloud compute networks create temtprivatenet --subnet-mode=custom
  109  gcloud compute networks create temtprivatenet2 --subnet-mode=custom
  110  clear
  111  gcloud compute networks subnet create temtprivatesubnet --network=temtprivatenet2 --region=us-west1 --range=10.0.0.0/24 --enable-private-ip-google-access
  112  gcloud compute networks subnets create temtprivatesubnet --network=temtprivatenet2 --region=us-west1 --range=10.0.0.0/24 --enable-private-ip-google-access
  113  clear
  114  gcloud compute instances create default-us-vm --zone=us-west1-a --network=default
  115  gcloud compute instances create mynet-us-vm --zone=us-west1-a --network=temtmynetwork
  116  gcloud compute instances create mynet-us-vm --zone=us-west1-a --network=temtnetwork
  117  gcloud compute instances create mynet-eu-vm --zone=us-central1-b --network=temtnetwork
  118  gcloud compute instances create privatenet-bastion --zone=us-west1-b --subnet=temtprivatesubnet2 --can-ip-forward
  119  gcloud compute instances create privatenet-bastion --zone=us-west1-b --subnet=temtprivatesubnet --can-ip-forward
  120  gcloud compute instances create privatenet-us-vm --zone=us-west1-c --subnet=privatesubnet
  121  gcloud compute instances delete tem-web-1 --keep-disks all
  122  gcloud compute images create temt-web-image --source-disk tem-web-1
  123  gcloud compute images create temt-web-image --source-disk tem-web-1c --source-disk-zone us-west1-a
  124  gcloud compute images create temt-web-image --source-disk tem-web-1 --source-disk-zone us-west1-a
  125  gcloud compute images list
  126  gcloud compute instances create temt-web-1 --image temt-web-image
  127  gcloud compute instances create temt-web-1 --image temt-web-image --zone=us-west1-a --subnet=privatesubnet
  128  gcloud compute instances create temt-web-1 --image temt-web-image --zone=us-west1-a --subnet=temtprivatesubnet
  129  gcloud compute ssh root@temt-web-1 --zone us-west1-a
  130  ip=$(curl -s https://api.ipify.org)
  131  echo $ip
  132  gcloud compute firewall-rules create temtnetwork-ingress-allow-ssh --network temtprivatenet2 --action ALLOW --direction INGRESS --rules tcp:22 --source-ranges 0.0.0.0/0 --target-tags=temt-web-ssh
  133  gcloud compute instances add-tags temt-web-1 --zone us-west1-a --tags temt-web-ssh
  134  gcloud compute instances add-tags mynet-eu-vm --zone europe-west1-b --tags temt-web-ssh
  135  gcloud compute instances add-tags mynet-eu-vm --zone europe-central1-b --tags temt-web-ssh
  136  gcloud compute instances add-tags mynet-eu-vm --zone us-central1-b --tags temt-web-ssh
  137  gcloud compute ssh test@mynet-eu-vm --zone us-central1-b
  138  gcloud compute ssh root@temt-web-1 --zone us-west1-a
  139  history 50
  140  gcloud compute instances add-tags privatenet-bastion --zone us-west1-b --tags temt-web-ssh
  141  clear
  142  gcloud compute firewall-rules create temtnetwork-ingress-deny-icmp-all --network temtprivatenet2 --action DENY --direction INGRESS --rules icmp --priority 500
  143  gcloud compute firewall-rules update temtnetwork-ingress-deny-icmp-all  --priority 2000
  144  gcloud compute firewall-rules create temtnetwork-ingress-allow-icmp-all --network temtprivatenet2 --action ALLOW --direction INGRESS --rules icmp --source-ranges 10.0.0.0/24
  145  gcloud compute firewall-rules list --filter="network:temt"
  146  gcloud compute firewall-rules list
  147  gcloud compute firewall-rules list --filter="network:temtprivatenet2"
  148  gcloud compute firewall-rules create temtnetwork-egress-deny-icmp-all --network temtprivatenet2 --action DENY --direction EGRESS --rules  icmp --priority 10000
  149  gcloud compute firewall-rules list --filter="network:temtprivatenet2"
  150  gcloud compute networks subnets update temtprivatenet2 --region us-west1 --enable-flow-logs
  151  gcloud compute networks subnets update temtprivatesubnet --region us-west1 --enable-flow-logs
  152  gcloud compute networks subnets pribatesubnet --region us-west1 --no-enable-flow-logs
  153  gcloud compute networks subnets temtprivatesubnet --region us-west1 --no-enable-flow-logs
  154  gcloud compute networks subnets update temtprivatesubnet --region us-west1 --no-enable-flow-logs
