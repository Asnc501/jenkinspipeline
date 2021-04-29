pipeline {
    agent any

    tools {
        maven 'maven 3.0.3'
	jdk 'localJDK'
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
                build job: 'deploy_to_staging'
            }
        }

        stage ('Deploy to Production'){
            steps{
                timeout(time:300, unit:'SECONDS'){
                    input message:'Do you Approve PRODUCTION Deployment?'
                }

                build job: 'deploy_to_prod'
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
