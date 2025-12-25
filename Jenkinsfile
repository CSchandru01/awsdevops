pipeline{
    agent any

    stages {
        stage ('checkout'){
            steps{
                checkout scm
            }
            env {
                SONAR_HOST_URL = 'http://localhost:9000'               
        }
        stage ('Code scan'){
            steps{
                withCredentials([string(credentialId: 'sonar-tocken', variable: 'SONAR_TOCKEN')]){
                    sh '''
                     sonar-scanner \
                      -Dsonar.projectKey=my-project \
                      -Dsonar.sources=. \
                      -Dsonar.host.url=$SONAR_HOST_URL \
                      -Dsonar.login=$SONAR_TOCKEN
                    '''
            }
            }
        }

        stage ('Quality Gate'){
            steps{
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage ('Build'){
            steps {
                sh './app.sh'

            }
        }

    }

}