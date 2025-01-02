pipeline {
    agent any
    tools {
        maven 'mvn-3.9.9'
    }
    stages {
        stage('Git') {
            steps {
                sh 'git branch: 'main', credentialsId: 'Gautam_GitHub_Creds', url: 'https://github.com/Learning-DevSecOps/Ekart.git''
            }
        }
        stage('mvn') {
            steps {
                sh 'mvn clean compile -DskipTests=true'
            }
        }        
    }
}