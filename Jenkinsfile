pipeline {
    agent {
        node {
            label "Dev"
        }
    }
    stages {
        stage('CleanWS') {
            steps {
                cleanWs()
            }
        }
        stage('CQA'){
            steps {
                withSonarQubeEnv('SonarQube') {
                  sh "./gradlew clean build sonar -Dsonar.projectKey=adservice"
                    }
                }
        }
        stage ('Build') {
            steps {
                sh './gradlew clean build'
                sh ''
            }
        }
        stage ('Artifact'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'hipstershop', classifier: '', file: 'build/libs/hipstershop-0.1.0-SNAPSHOT.jar', type: 'war']], credentialsId: 'NexusCred', groupId: 'adservice', nexusUrl: '54.224.94.208:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'adservice-svc', version: '0.1.0-SNAPSHOT'
            }
        }
        stage ('DockerBuild'){
            steps {
                sh 'docker build -t devsecopsdh/eksmsproject:adservice'
        
            }
        }
        stage ('TrivyScan'){
            steps {
                sh 'trivy image devsecopsdh/eksmsproject:adservice'
            }
        }
        stage ('DockerPush'){
            steps {
                withDockerRegistry(credentialsId: 'DockerHub') {
                       sh 'docker push devsecopsdh/eksmsproject:adservice'
                       
                     }
            }
        }
    }
}
