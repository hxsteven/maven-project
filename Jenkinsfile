pipeline {
    agent any
    tools {
        maven 'LocalMaven'
        jdk 'LocalJDK'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '35.182.52.19', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '35.183.5.86', description: 'Production Server')
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
                        bat "C:\\'Program Files'\\Git\\usr\\bin\\scp.exe -i /c/Users/haixin/NewKeyPair.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }
                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /c/Users/haixin/NewKeyPair.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}