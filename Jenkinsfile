pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '35.166.210.154', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.209.233.6', description: 'Production Server')
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
                    archiveArtifacts artifacts: 'c:\Users\Zsolt_Nyerges\Downloads\apache-tomcat-8.5.40\webapps\*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i  *c:\Users\Zsolt_Nyerges\Downloads\AWS\Keypairs\tomcat-demo.pem c:\Users\Zsolt_Nyerges\Downloads\apache-tomcat-8.5.40\webapps\*.war centos@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i c:\Users\Zsolt_Nyerges\Downloads\AWS\Keypairs\tomcat-demo.pem c:\Users\Zsolt_Nyerges\Downloads\apache-tomcat-8.5.40\webapps\*.war centos@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
