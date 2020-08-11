pipeline {
  agent {
    docker {
      image "bryandollery/terraform-packer-aws-alpine"
      args "-u root"
    }
  }
  environment {
    CREDS = credentials('aws-creds')
    AWS_ACCESS_KEY_ID = "${CREDS_USR}"
    AWS_SECRET_ACCESS_KEY = "${CREDS_PSW}"
    OWNER = "bryan"
    PROJECT_NAME = 'web-server'
    AWS_PROFILE="kh-labs"
    TF_NAMESPACE="bryan"
  }
  stages {
      stage("init") {
          steps {
              make init
          }
      }
      stage("workspace") {
          steps {
              sh """
terraform workspace select jenkins-lab-2
if [[ $? -ne 0 ]];
  terraform workspace new jenkins-lab-2
fi
"""
          }
      }
      stage("plan") {
          steps {
              make plan
          }
      }
      stage("apply") {
          steps {
              make apply
          }
      }
  }
}