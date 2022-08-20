pipeline {
    
    agent none
    tools {
        maven 'Maven3'
    }
    stages {
        
        stage('build') {
            agent {label 'slave'}
            steps {
                sh 'mvn clean package'
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
                deploy adapters: [tomcat9(credentialsId: 'tomcat-user', path: '', url: 'http://65.2.150.178:8080/')], contextPath: null, war: '**/*.war'
            }
        }
    }
}
