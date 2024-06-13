pipeline {
    agent any
    stages {
        stage('checkout') {
            steps {
                git branch: 'reminder',
                    url: 'https://github.com/Kvadrokom/Jenkins.git'
                }
                sh 'pwd'
                sh 'ls'
            }
        }
    post {
        failure{
            echo 'I will always say Hello only failure'
        }
        success {
            echo 'I will always say Hello only success'
        }
        always {
            echo 'I will always say Hello only again'
            cleanWs()
        }
    }
}
