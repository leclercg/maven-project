pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '54.89.90.101', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.89.90.101', description: 'Production Server')
        string(name: 'myKey', defaultValue: "C:/Users/Georges/Downloads/tomcat-demo.pem", description: 'Staging Server SSH Key')
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
                        bat "pscp -scp -i ${params.myKey} webapp/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
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
