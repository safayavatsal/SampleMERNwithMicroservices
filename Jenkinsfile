pipeline {
    agent any
    environment{
        AWS_ACCESS_KEY='aws_credencials'
        SSH_KEY = credentials('d2250590-e41c-4157-9756-95f9ba817f08')
        DOCKER_HUB_CREDENTIALS= 'docker-hub-credentials'
        DOCKER_FRONTEND = 'flowerking21/learnersreport_frontservice'
        DOCKER_BACKEND_HELLO = 'flowerking21/micro_backend_helloservice'
        DOCKER_BACKEND_PROFILE = 'flowerking21/micro_backend_profileservice'
       
    }

    stages {
        
        stage("login"){
            steps{
                withCredentials([aws(credentialsId: 'aws_credencials', region:'ap-south-1')]) {
                        echo "login success"
                }
            }
        }
        
        stage('CHECKOUT') {
            steps {
                echo 'clone the git code' 
                git branch: 'main', url:'https://github.com/flowerpoo/Orchestration_and_Scaling.git'
            }
        }
        
        stage("build image"){
            parallel{
                stage("build backend_hello service"){
                    steps{
                        script{
                            sh 'cd backend/helloservice && docker build -t 515210271098.dkr.ecr.ap-south-1.amazonaws.com/micro_backendhello:latest .'
                        }
                    }
                }
                
                stage("build backend_profile service"){
                    steps{
                        script{
                            sh 'cd backend/profileservice && docker build -t 515210271098.dkr.ecr.ap-south-1.amazonaws.com/micro_backendprofile:latest .'
                        }
                    }
                }
                
                stage("build frontend"){
                    steps{
                        script{
                            sh 'cd frontend && docker build -t 515210271098.dkr.ecr.ap-south-1.amazonaws.com/micro_frontendservice:latest .'
                        }
                    }
                }
            }
           
        }
        
        stage('ECR push'){
            steps{
                script{
                    sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 515210271098.dkr.ecr.ap-south-1.amazonaws.com'
                    sh 'docker push 515210271098.dkr.ecr.ap-south-1.amazonaws.com/micro_backendhello:latest'
                    sh 'docker push 515210271098.dkr.ecr.ap-south-1.amazonaws.com/micro_backendprofile:latest'
                    sh 'docker push 515210271098.dkr.ecr.ap-south-1.amazonaws.com/micro_frontendservice:latest'
                }
            }
        }
        
        
     
        
        
        
    }
}
