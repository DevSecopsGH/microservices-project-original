pipeline {
    agent any

    stages {
        stage('Deploy To Kubernetes') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'DevSecopsEKS-2', contextName: '', credentialsId: 'k8-token', namespace: 'webapp', serverUrl: 'https://E3008C629102E40870C69A8AE82B738C.gr7.us-east-1.eks.amazonaws.com']]) {
                    sh "kubectl apply -f deployment-service.yml"
                    
                }
            }
        }
        
        stage('verify Deployment') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'DevSecopsEKS-2', contextName: '', credentialsId: 'k8-token', namespace: 'webapp', serverUrl: 'https://E3008C629102E40870C69A8AE82B738C.gr7.us-east-1.eks.amazonaws.com']]) {
                    sh "kubectl get svc -n webapp"
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
