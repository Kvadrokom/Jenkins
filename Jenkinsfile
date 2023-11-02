pipeline {
    agent any
    options {
        timestamps()
        ansiColor('xtera')
    }
    stages {
        stage('example matrix') {
            matrix {
                axes {
                    axis {
                        name 'OS'
                        values 'linux', 'chrome', 'safari'
                    }
                    axis {
                        name 'BROWSERS'
                        values 'firefox', 'chrome', 'safari'
                    }
                }
                exludes {
                    exlude {
                        axis {
                            name 'OS'
                            values 'linux'
                        }
                        axis {
                            name 'BROWSERS'
                            value 'safari'
                        }
                    }
                }
                stages {
                    stage('Matrix hello') {
                        steps {
                            echo ' hello world from ${OS} ${BROWSERS}'
                        }
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
