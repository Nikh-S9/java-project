pipeline {
    
    agent none
    tools {
        maven 'Maven3'
    }
    stages {
        stage('Checkout') {
            agent {label 'slave'}
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Nikh-S9/java-project.git']]])
            }
        }
        
        stage('build') {
            agent {label 'slave'}
            steps {
                sh 'mvn clean install'
            }
            post {
                success {
                    sh 'echo "archiving the artifacts"'
                    archiveArtifacts artifacts:'**/target/*.war'
                }
            }
        }
        
        stage('Deploy to tomcat server') {
            agent {label 'slave'}
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat-user', path: '', url: 'http://13.126.127.245:8080/')], contextPath: null, war: '**/*.war'
            }
        }
    }
}
