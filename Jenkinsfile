pipeline {
    agent any
    tools {
        jdk 'jdk11'
        maven 'mvn-3.9.9'
        dockerTool 'docker'
        git 'Default'
    }
    environment {
        DOCKER_HUB_REPO = 'gautam1998/ekart'
        DOCKER_HUB_CREDS = credentials('docker_cred')
    }
    stages {
        stage('Git Checkout') {
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
        stage('OWASP Scan') {
            steps {
                dependencyCheck(
                    failBuildOnCVSS: 7,
                    format: 'XML',
                    outputDirectory: 'dependency-check-report',
                    scanPath: '**/target/*.jar'
                )
            }
        }
    }
}
