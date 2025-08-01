pipeline {
agent any
    triggers {
githubPush()
    }
 environment {
  EC2_IP = '44.204.3.77' // Replace with your EC2 public IP SSH_CRED = 'website-ec2-key' // Jenkins SSH credential ID
 REPO = 'https://github.com/Devivaraprasadreddy/portfolioreactwithjenkins.git' // Your React portfolio Git repo
APP_DIR = '/home/ubuntu/portfolio-app' // Target directory on EC2
 
  }
  stages {
    stage('Trigger Received'){
        steps{
            echo 'GitHub Push Triggered. Starting React Portfolio Deployment Pipeline ....'
        }
    }
    stage('Pull React App on EC2') {
  steps {
sshagent (credentials: ["ec2"]) {
  sh """
ssh -o StrictHostKeyChecking=no ubuntu@${EC2_IP} '
  rm -rf ${APP_DIR} && \
git clone ${REPO} ${APP_DIR}
  ' """
 }
}
}
stage('Install Dependencies and Build React App') {
  steps {
sshagent (credentials: ["ec2"]) {
  sh """
 
ssh -o StrictHostKeyChecking=no ubuntu@${EC2_IP} '
cd ${APP_DIR} && \
  npm install && \
npm run build
' """
   }
  }
 }

stage('Start App with PM2') {
steps {
  sshagent (credentials: ["ec2"]) {
sh """
  ssh -o StrictHostKeyChecking=no ubuntu@${EC2_IP} '
cd ${APP_DIR} && \
  pm2 delete portfolio || true && \
pm2 start npm --name "portfolio" -- start
' """
  }
   }
   }

  }
 
 
  
    }
 