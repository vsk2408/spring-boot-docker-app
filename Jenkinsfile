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
              sh 'docker save -o spring-boot-app.tar spring-boot-app:latest'
              sh 'chown jenkins:jenkins spring-boot-app.tar'
          }
       }
       stage('ansible_playbook'){
           steps{
               sh ''' chmod +X ansible.yml
                     ansible-playbook -i /etc/ansible/inventory ansible.yml'''
           }
       }
    }
}    
