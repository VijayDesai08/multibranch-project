pipeline {
    agent any
    tools {
        maven 'maven-3.9.9'
    }

    stages {
        stage('SCM') {
            steps {
                git branch: "${env.BRANCH_NAME}", credentialsId: 'git', url: 'https://github.com/VijayDesai08/demo-project1.git'
            }
        }
        
        stage('BUILD WAR FILE') {
            steps {
                sh '/opt/maven-3.9.9/bin/mvn clean install'
            }
        }
        
        stage('BUILD IMAGE') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred') {
                        sh "docker build -t vijay008/demo-project1-${env.BRANCH_NAME}:v$BUILD_NUMBER ."
                    }
                }
            }
        }
        
        stage('SCAN IMAGE') {
            steps {
                sh "trivy image vijay008/demo-project1-${env.BRANCH_NAME}:v$BUILD_NUMBER >> image-${env.BRANCH_NAME}.txt"
            }
        }
        
        stage('PUSH IMAGE') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred') {
                        sh "docker push vijay008/demo-project1-${env.BRANCH_NAME}:v$BUILD_NUMBER"
                    }
                }
            }
        }
        
        stage('DEPLOY IMAGE') {
            steps {
                sh "docker stop demo-cont-${env.BRANCH_NAME} || true"
                sh "docker run --rm --name demo-cont-${env.BRANCH_NAME} -d -p 8082:8080 vijay008/demo-project1-${env.BRANCH_NAME}:v$BUILD_NUMBER"
            }
        }
    }
}
