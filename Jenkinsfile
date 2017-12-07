pipeline {
    agent any

    parameters {
	string(name: 'tomcat_prod', defaultValue: '140.251.6.145', description: 'Production Server')
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
        stage ('Deploy to Staging'){
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage ('Deploy to Production'){
            steps{
		sh "scp **/target/*.war tomcatuser@${params.tomcat_prod}:/usr/local/apache-tomcat/webapps/"
                }

            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }


    }
}
