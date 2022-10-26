pipeline {
    agent any
    tools {
        maven 'maven_3.8.6'
    }

    stages {
        stage('Build Maven') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/YasserImam/devops_automation']]])
                bat 'mvn clean install'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    bat 'docker build -t yasserimam/devops-integration .'
                }
            }
        }
        stage('Push Image to Hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        bat 'docker login -u yasserimam -p %dockerhubpwd%'
                    }
                    //bat 'docker tag demo/devops-integration:latest yasserimam/demo-repo:firstpush'
                    bat 'docker push yasserimam/devops-integration'
                }
            }

        }
    }
}
