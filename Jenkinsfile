pipeline {
    agent any
    tools {
        maven 'LocalMaven'
        jdk 'LocalJDK'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '35.182.95.12', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '35.183.37.248', description: 'Production Server')
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
                        bat '"C://Program Files (x86)//WinSCP/WinSCP.exe" -i /home/jenkins/NewKeyPair.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps'
                    }
                }
                stage ("Deploy to Production"){
                    steps {
                        bat "WinSCP.exe -i /home/jenkins/NewKeyPair.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}