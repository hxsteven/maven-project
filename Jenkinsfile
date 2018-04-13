pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                echo 'Building'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
    }
}