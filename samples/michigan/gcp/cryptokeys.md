  293  echo $DEVSHELL_PROJECT_ID
  294  gsutil mb -l us gs://$DEVSHELL_PROJECT_ID-kms
  295  echo "This is sample file 1" > file1.txt
  296  echo "This is sample file 2" > file2.txt
  297  echo "This is sample file 3" > file3.txt
  298  gsutil cp file1.txt gs://$DEVSHELL_PROJECT_ID-kms
  299  gscloud services enable cloudkms.googleapis.com
  300  gcloud services enable cloudkms.googleapis.com
  301  KEYRING_NAME=lab-keyring
  302  CRYPTOKEY_1_NAME=labkey-1
  303  CRYPTOKEY_2_NAME=labkey-2
  304  gcloud kms keyrings create $KEYRING_NAME --location global
  305  gcloud kms keys create $CRYPTOKEY_1_NAME --location global --keyring $KEYRING_NAME --purpose exncryption
  306  gcloud kms keys create $CRYPTOKEY_1_NAME --location global --keyring $KEYRING_NAME --purpose encryption
  307  gcloud kms keys create $CRYPTOKEY_2_NAME --location global --keyring $KEYRING_NAME --purpose encryption
  308  gsutil kms encryption gs://$DEVSHELL_PROJECT_ID-kms
  309  gsutil kms authorize -p $DEVSHELL_PROJECT_ID -k projects/$DEVSHELL_PROJECT_ID/locations/global/keyRings/$KEYRING_NAME/cryptoKeys/$CRYPTOKEY_1_NAME
  310  gsutil kms authorize -p $DEVSHELL_PROJECT_ID -k projects/$DEVSHELL_PROJECT_ID/locations/global/keyRings/$KEYRING_NAME/cryptoKeys/$CRYPTOKEY_2_NAME
  311  gsutil kms encryption -k projects/$DEVSHELL_PROJECT_ID/locations/global/keyRings/$KEYRING_NAME/cryptoKeys/$CRYPTOKEY_1_NAME gs://$DEVSHELL_PROJECT_ID-kms
  312  gsutil kms encryption gs://$DEVSHELL_PROJECT_ID-kms
  313  gsutil cp file2.txt gs://$DEVSHELL_PROJECT_ID-kms
  314  gsutil -o "GSUtil:encryption_key=projects/$DEVSHELL_PROJECT_ID/locations/global/keyRings/$KEYRING_NAME/cryptoKeys/$CRYPTOKEY_2_NAME" cp file3.txt gs://$DEVSHELL_PROJECT_ID-kms
  315  gsutil ls -L gs://$DEVSHELL_PROJECT_ID-kms/file3.txt
  316  gsutil ls -L gs://$DEVSHELL_PROJECT_ID-kms/file2.txt
  317  gsutil ls -L gs://$DEVSHELL_PROJECT_ID-kms/file1.txt
  318  PLAIN_TEXT=$(echo -n "Some text to be encrypted" | base64)
  319  echo $PLAIN_TEXT
  320  curl "https://cloudkms.googleapis.com/v1/projects/$DEVSHELL_PROJECT_ID/locations/global/keyRings/$KEYRING_NAME/cryptoKeys/$CRYPTOKEY_1_NAME:encrypt" -d "{\"plaintext\":\"$PLAIN_TEXT\"}" -H "Authorization:Bearer $(gcloud auth application-default print-access-token)" -H "Content-Type: application/json"
  321  curl "https://cloudkms.googleapis.com/v1/projects/$DEVSHELL_PROJECT_ID/locations/global/keyRings/$KEYRING_NAME/cryptoKeys/$CRYPTOKEY_1_NAME:encrypt" -d "{\"plaintext\":\"$PLAIN_TEXT\"}" -H "Authorization:Bearer $(gcloud auth application-default print-access-token)" -H "Content-Type: application/json" | jq .ciphertext -r > data1.encrypted
  322  ls
  323  mote data1.encrypted
  324  more data1.encrypted
  325  curl -v "https://cloudkms.googleapis.com/v1/projects/$DEVSHELL_PROJECT_ID/locations/global/keyRings/$KEYRING_NAME/cryptoKeys/$CRYPTOKEY_1_NAME:decrypt" -d "{\"ciphertext\":\"$(cat data1.encrypted)\"}" -H "Content-Type: application/json" | jq .plaintext -r | base64 -d > data1.decrypted
  326  more data1.decrypted
  327  curl -v "https://cloudkms.googleapis.com/v1/projects/$DEVSHELL_PROJECT_ID/locations/global/keyRings/$KEYRING_NAME/cryptoKeys/$CRYPTOKEY_1_NAME:decrypt" -d "{\"ciphertext\":\"$(cat data1.encrypted)\"}" -H "Content-Type: application/json"
  328  curl -v "https://cloudkms.googleapis.com/v1/projects/$DEVSHELL_PROJECT_ID/locations/global/keyRings/$KEYRING_NAME/cryptoKeys/$CRYPTOKEY_1_NAME:decrypt" -d "{\"ciphertext\":\"$(cat data1.encrypted)\"}" -H "Content-Type: application/json" -H "Authorization:Bearer $(gcloud auth application-default print-access-token)"  | jq .plaintext -r | base64 -d > data1.decrypted
  329  more data1.decrypted
