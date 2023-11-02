pipeline {
    agent any
    options {
            timestamps()
            ansiColor('xtera')
    }
    stages {
        stage('init') {
            steps {
                echo 'init Hello world!!'
            }
        }
        stage('example parallel') {
        parallel {
            stage('chrome') {
                steps {
                    echo 'Hello from Chrome!!!'
                    }
                }
            stage('firefox') {
                steps {
                    echo 'Hello from Firefox!!!'
                    }
                }
            }
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
