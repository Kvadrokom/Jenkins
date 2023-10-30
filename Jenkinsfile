pipeline {
    agent any
    options {
        timestamps()
        ansicolor('xtera')
    }
    stages {
        stage('example') {
            steps {
                echo 'Hello world!!'
                script {
                    def browsers = ['chrome', 'firefox']
                    for (int i = 0; i < browsers.size(); ++i)
                        echo "testing the ${ 'browsers' } browser"
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
