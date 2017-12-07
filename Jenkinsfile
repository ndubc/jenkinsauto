pipeline {
    agent any


    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh '/opt/apache-maven-3.5.2/bin/mvn clean package'
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
			build job: 'deploy-to-staging'
                    }
                }

                stage ("Deploy to Production"){
                    steps {
			sh "scp/var/lib/jenkins/workspace/package/webapp/target/*.war tomcatuser@140.251.6.145:/usr/local/apache-tomcat/webapps/"
                    }
                }
            }
        }
    }
}
