pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '54.89.90.101', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.89.90.101', description: 'Production Server')
         string(name: 'myKey', defaultValue: "C:/Users/Georges/Downloads/tomcat-demo-ppk.ppk", description: 'Staging Server SSH Key')
         string(name: 'myHostKey', defaultValue: "fd:ec:ec:dd:60:d4:c3:5b:2d:5d:f1:d0:63:37:c3:e9", description: 'Host Key')
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "pscp -batch -hostkey ${params.myHostKey} -i ${params.myKey} webapp/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        echo 'Deloying to prod for fake'
                    }
                }
            }
        }
    }
}
