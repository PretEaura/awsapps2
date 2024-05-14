pipeline {
    agent any
    environment {
        // AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        // AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        // AWS_DEFAULT_REGION = 'us-east-1'
        TAG =1.0
    }
    stages{
        stage('Checkout SCM'){
            steps{
                script{
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/PretEaura/myaws2.git']])
                }
            }
        }
        stage ('build'){
            steps{
                script{
                    sh 'docker build . -t 211125773797.dkr.ecr.ap-south-1.amazonaws.com/my-ecr-repo/image:$TAG'
                }
            }
        }
        stage ('push'){
            steps{
                script{
                    sh 'docker push 211125773797.dkr.ecr.ap-south-1.amazonaws.com/my-ecr-repo/image:$TAG'
                }
            }
        }
        stage('apply deploy'){
            steps{
                script{
                    sh ``` kubectl apply -f deployment.yaml | 
                     kubectl apply -f service.yaml |
                     kubectl apply -f ingress.yaml |
                     kubectl apply -f ingress-controller.yaml 
                     ```
                    }
                }
            }
        }
    }
