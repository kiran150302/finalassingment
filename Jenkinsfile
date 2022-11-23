@Library('github.com/releaseworks/jenkinslib') _

pipeline {
    agent any
    environment {
        registry = "561358984168.dkr.ecr.us-east-1.amazonaws.com/kiranassingment-ecr"
    }

    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/kiran150302/finalassingment.git']]])
            }
        }

    // Building Docker images


    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
        steps{
            script {
                sh 'docker build -t 561358984168.dkr.ecr.us-east-1.amazonaws.com/kiranassingment-ecr . '
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 561358984168.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker push 561358984168.dkr.ecr.us-east-1.amazonaws.com/kiranassingment-ecr'
            }
        }
    }

    
    stage('Docker Run') {
     steps{
         script {
             sshagent(credentials : ["9c8bb63f-0757-4d77-bcda-ec8cb8682682"]){

                sh 'ssh -t -t ubuntu@10.0.2.179 -o StrictHostKeyChecking=no "docker login -u AWS -p $(aws ecr get-login-password --region us-east-1) 561358984168.dkr.ecr.us-east-1.amazonaws.com && docker run -d -p 8080:8081 --rm --name nodeapp 561358984168.dkr.ecr.us-east-1.amazonaws.com/kiranassingment-ecr:latest"'

             }
                
                
            }
      }
    }
    }
}
