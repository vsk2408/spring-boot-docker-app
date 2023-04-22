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
       stage('Static Code Analysis') {
            environment {
                SONAR_URL = "http://15.206.212.53:9000/"
            }
            steps {
                 withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
                 sh 'mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
        }
      }
    }
}    
