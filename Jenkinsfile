pipeline {
    agent any
    options {
        timestamps()
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
             ansiColor('xterm') {
                    echo '\033[32mGreen text!\033[0m'
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
