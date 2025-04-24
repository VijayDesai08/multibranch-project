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
    }
}
