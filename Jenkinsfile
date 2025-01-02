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
                script {
                    // Build the Docker image
                    docker.build("${DOCKER_HUB_REPO}:${BUILD_NUMBER}")
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                    // Login to DockerHub
                    sh "echo ${DOCKER_HUB_CREDS_PSW} | docker login -u ${DOCKER_HUB_CREDS_USR} --password-stdin"
                    
                    // Push the Docker image
                    sh "docker push ${DOCKER_HUB_REPO}:${BUILD_NUMBER}"
                    
                    // Push latest tag
                    sh "docker tag ${DOCKER_HUB_REPO}:${BUILD_NUMBER} ${DOCKER_HUB_REPO}:latest"
                    sh "docker push ${DOCKER_HUB_REPO}:latest"
                    
                    // Cleanup
                    sh "docker rmi ${DOCKER_HUB_REPO}:${BUILD_NUMBER}"
                    sh "docker rmi ${DOCKER_HUB_REPO}:latest"
                }
            }
        }             
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}