pipeline {
    agent any
    tools {
             maven 'maven'
    }

    stages {
        stage('SCM') {
            steps {
                git credentialsId: 'github', url: 'https://github.com/veelash27/dockeransiblejenkins.git'
            }
        }
        stage('Maven Build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage('Docker  Build') {
            steps {
                sh "docker build . -t veelashpat/pipeline:0.0.1"
            }
        }
        stage('Dockerhub  push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'password', usernameVariable: 'username')]) {
                sh "docker login -u ${username} -p ${password}"
            }
                sh "docker push veelashpat/pipeline:0.0.1"
            }
        }
        stage('Docker  deploy') {
            steps {
            ansiblePlaybook credentialsId: 'dev-server1', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml'
            }
        }
        
    }
}
