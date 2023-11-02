pipeline {
    node {
            BRANCH_NAMES = sh (script: 'git ls-remote -h https://github.com/Kvadrokom/Jenkins.git | sed \'s/\\(.*\\)\\/\\(.*\\)/\\2/\' ', returnStdout:true).trim()
    }
    agent any
    parameters {
            choice(
                name: 'BranchName',
                choices: "${BRANCH_NAMES}",
                description: 'to refresh the list, go to configure, disable "this build has parameters", launch build (without parameters)to reload the list and stop it, then launch it again (with parameters)'
            )
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
