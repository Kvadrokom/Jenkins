pipeline {
    agent any
    stages {
        stage('checkout1') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Kvadrokom/reminder.git'
                sh 'pwd'
                sh 'ls'
                }
            }
         stage('checkout2') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Kvadrokom/AnsibleDeploy.git'
                sh 'pwd'
                sh 'ls'
                ansiblePlaybook('reminder.yml') {
                  inventoryPath('hosts.ini')
                  credentialsId('Test_ssh_key_deploy_reminder')
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
