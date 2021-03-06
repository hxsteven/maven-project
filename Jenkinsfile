pipeline {
    agent any
    tools {
        maven 'localMaven'
        jdk 'localJDK'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '35.183.35.39', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '35.183.3.149', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
    }

    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
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
                        sh "scp -i /home/NewKeyPair.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }
                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/NewKeyPair.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}