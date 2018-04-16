pipeline {
    agent any
    tools {
        maven 'LocalMaven'
        jdk 'LocalJDK'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'localhost:8090', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost:9090', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
    }

    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deployments') {
            parallel {
                stage ('Deploy to Staging'){
                    steps {
                        sh "cp **/target/*.war tomcat:tomcat@${params.tomcat_dev}:/webapps"
                    }
                }
                stage ("Deploy to Production"){
                    steps {
                        sh "cp **/target/*.war tomcat:tomcat@${params.tomcat_prod}:/webapps"
                    }
                }
            }
        }
    }
}