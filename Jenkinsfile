pipeline {
    agent any
    tools {
        maven 'LoalMaven'
        jdk 'LocalJDK'
    }
    stages{
        stage('Build'){
            steps {
                Bat "mvn clean package"
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}