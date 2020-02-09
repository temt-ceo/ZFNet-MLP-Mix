  350  export GCLOUD_PROJECT=$DEVSHELL_PROJECT_ID
  351  git clone https://github.com/googleapis/nodejs-dlp.git
  352  cd nodejs-dlp/samples/
  353  ls
  354  npm install @google-cloud/dlp
  355  npm install yargs
  356  npm install mime
  357  node inspect.js string "My email address is joe@example.com."
  358  node inspect.js string "My phone number is 555-555-5555."
  359  node inspect.js string "My secret number is 1234-5678-9876-5432."
  360  node inspect.js string "My secret number is 123-45-6789."
  361  node deid.js deidMask "My phone number is 555-555-5555."
  362  node redact.js image ~/dlp-input.png dlp-redacted.png -t DOMAIN_NAME
  363  echo $DOMAIN_NAME
  364  node redact.js image ~/dlp-input.png dlp-redacted2.png -t PHONE_NUMBER
