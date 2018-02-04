pipeline {
    agent any
    stages {
        
        stage ('Build'){
            steps{
                mvn clean package
            }
            post {
                success{
                    echo 'Now Archiving ....'
                    archiveArtifacts artifacts: '**/.war'
                }
            }
        }
        
        stage ('Deploy-to-staging'){
            steps{
                build job: 'testapp-deploy-to-staging'
            }
        }

        stage ('Deploy-to-production'){
            steps{
                timeout(time: 5, units: 'DAYS'){
                    input message: 'Approve PRODUCTION deployment?'
                }
                build job: 'testapp-deploy-to-prod'
            }
            post {
                success{
                    echo ' Deployed build to production. '
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
    }
}