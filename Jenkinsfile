pipeline {
  agent any
  
  options{
        buildDiscarder(logRotator(daysToKeepStr: '7'))
        disableConcurrentBuilds()
        retry(3)
        timeout(time: 1, unit: 'HOURS')

    }
    parameters{
        string(name: 'BRANCH', defaultValue: 'main')
    }

  stages {
    stage('Stage 1: Git Checkout') {
      steps {
        checkout scm
      }
    }
  }
    stage('Stage 2: Build Docker Image') {
      parallel {
        stage('Build Docker Image') {
          steps {
            sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 609042131998.dkr.ecr.us-east-1.amazonaws.com'
            sh 'sudo docker build -t hello-nodejs .'
            sh 'sudo docker tag hello-nodejs:latest 609042131998.dkr.ecr.us-east-1.amazonaws.com/hello-nodejs:latest'
            sh 'sudo docker push 609042131998.dkr.ecr.us-east-1.amazonaws.com/hello-nodejs:latest'
          }
        }
      }
    }
}
