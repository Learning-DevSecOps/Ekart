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
                git branch: 'main', credentialsId: 'Gautam_GitHub_Creds', url: 'https://github.com/Learning-DevSecOps/Ekart.git'
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
        stage('Docker Build') {
            steps {
                sh """
                    docker build -t ${DOCKER_HUB_REPO}:${BUILD_NUMBER} ./docker/
                    docker tag ${DOCKER_HUB_REPO}:${BUILD_NUMBER} ${DOCKER_HUB_REPO}:latest
                """
            }
        }
        stage('Docker Push') {
            steps {
                sh """
                    echo ${DOCKER_HUB_CREDS_PSW} | docker login -u ${DOCKER_HUB_CREDS_USR} --password-stdin
                    docker push ${DOCKER_HUB_REPO}:${BUILD_NUMBER}
                    docker push ${DOCKER_HUB_REPO}:latest
                    docker rmi ${DOCKER_HUB_REPO}:${BUILD_NUMBER}
                    docker rmi ${DOCKER_HUB_REPO}:latest
                """
            }
        }             
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}