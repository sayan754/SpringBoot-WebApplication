pipeline {
    agent any
    
    tools {
        jdk 'jdk11'
        maven 'maven3'
    }
    
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout ') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/jaiswaladi246/SpringBoot-WebApplication.git'
            }
        }
        
        stage('Code Compile') {
            steps {
                    sh "mvn compile"
            }
        }
        
        stage('Run Test Cases') {
            steps {
                    sh "mvn test"
            }
        }
        stage('SonarQube Analysis') {
            steps {
                    withSonarQubeEnv(credentialsId: '9eebb8aa-91ee-4d57-8250-ce7ed9390b1c') {
                        sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Java-WebApp \
                        -Dsonar.java.binaries=. \
                        -Dsonar.projectKey=Java-WebApp '''
    
                }
            }
        }
        stage('Maven Build') {
            steps {
                    sh "mvn compile"
            }
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'd0c9cdc6-a9ae-4986-9689-44e9e8b88589') {
                       
                            sh "docker build -t webapp ."
                            sh "docker tag webapp sayan510/webapp:latest"
                            sh "docker push sayan510/webapp:latest " 
                    }
                }
            }
        }
        stage('Docker Image scan') {
            steps {
                    sh "trivy image sayan510/webapp:latest "
            }
        }
    
    }
}
