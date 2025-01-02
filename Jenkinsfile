pipeline {
    agent any
    tools {
        maven 'mvn-3.9.9'
        dockerTool 'docker'
        git 'Default'
    }
    environment {
        DOCKER_HUB_REPO = 'gautam1998/ekart'
        DOCKER_HUB_CREDS = credentials('docker_cred')
    // IMAGE_NAME = 'gautam1998/ekart'
    // IMAGE_TAG = "${IMAGE_NAME}:${env.BUILD_NUMBER}"
    // KUBECONFIG = credentials('kubeconfig-credentials-id')
    }
    stages {
        stage('Git') {
            steps {
                git branch: 'main', changelog: false, credentialsId: 'Gautam_GitHub_Creds', poll: false, url: 'https://github.com/Learning-DevSecOps/Ekart.git'
            }
        }
        stage('mvn') {
            steps {
                sh 'mvn clean compile -DskipTests=true'
            }
        }
        stage('mvn build') {
            steps {
                sh 'mvn clean package -DskipTests=true'
            }
        }
    }
}
