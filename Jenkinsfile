pipeline {
    agent any
    tools {
        maven 'mvn-3.9.9'
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
    }
}