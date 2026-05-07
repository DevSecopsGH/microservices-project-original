pipeline {
    agent any

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t devsecopsdh/paymentservice:latest ."
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push devsecopsdh/paymentservice:latest "
                    }
                }
            }
        }
    }
    post {
        always {
            slackSend channel: 'eksproject-1', message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} More info at:${env.BUILD_URL}"
        }
        
    }
}
