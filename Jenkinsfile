pipeline {
    agent {
        node {
            label "Dev"
        }
    }
    tools {
        maven 'kopsmaven'
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
                  sh "mvn clean verify sonar:sonar -Dsonar.projectKey=adservice"
                    }
                }
        }
        stage ('Build') {
            steps {
                sh 'mvn clean package'
                sh 'cp -r target Docker-app'
            }
        }
        stage ('Artifact'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'vprofile', classifier: '', file: 'target/vprofile-v2.war', type: 'war']], credentialsId: 'NexusCred', groupId: 'com.visualpathit', nexusUrl: '54.224.94.208:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'Docker-webapp', version: 'v2'
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
