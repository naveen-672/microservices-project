
pipeline {
    agent any

    tools {
        maven "myMaven"
    }

    stages {

        stage('CQA') {
            steps {
                withSonarQubeEnv("CQA_credentails") {
                    sh '''
                        mvn clean verify \
                        org.sonarsource.scanner.maven:sonar-maven-plugin:sonar \
                        -Dsonar.projectKey=micro-services \
                        -Dsonar.projectName="micro-services"
                    '''
                }
            }
        }

        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(
                        credentialsId: 'docker-cred',
                        toolName: 'docker'
                    ) {
                        sh 'docker build -t naveenkumar30/adservice:latest .'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(
                        credentialsId: 'docker-cred',
                        toolName: 'docker'
                    ) {
                        sh 'docker push naveenkumar30/adservice:latest'
                    }
                }
            }
        }
    }
}

