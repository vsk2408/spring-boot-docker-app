pipeline {
    agent any
    stages {
        stage('git_clone'){
            steps{
                git branch: 'main', url: 'https://github.com/vsk2408/spring-boot-docker-app.git'
            }
        }
        stage('build'){
           steps{
               sh 'mvn clean install -X -U'
           }
       }
       stage('docker_build'){
          steps{
              sh 'docker build -t spring-boot-app -f Dockerfile .'
              //sh 'docker save -o spring-boot-app.tar spring-boot-app:latest'
              //sh 'chown jenkins:jenkins spring-boot-app.tar'
          }
       }
       stage('Pushing Image') {
            steps {
                script {
                    sh '''aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 723185050974.dkr.ecr.ap-south-1.amazonaws.com
                    docker tag spring-boot-app:latest 723185050974.dkr.ecr.ap-south-1.amazonaws.com/sping-pet-clinic-repo:latest
                    docker push 723185050974.dkr.ecr.ap-south-1.amazonaws.com/sping-pet-clinic-repo:latest'''
                }
            }
       }
        stage('Deployment') {
            steps {
                sshagent(['K8s_Master']) {
                    sh 'scp -o StrictHostKeyChecking=no deployment.yaml ubuntu@13.233.161.200:/home/ubuntu/'
                    script {
                        try{
                            sh 'ssh ubuntu@13.233.161.200 kubectl apply -f .'
                        } catch(error){
                            sh 'ssh ubuntu@13.233.161.200 kubectl create -f .'
                        }
                    }
                }
            }
        }
    }
}    
