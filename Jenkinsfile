pipeline {
    agent any
    stages {
        stage('checkout1') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Kvadrokom/reminder.git'
                sh 'pwd'
                sh 'ls'
                sh 'whoami'
                sh 'mkdir ~/ansible && cp reminder.py reminder_utils.py ~/ansible'
                }
            }
         stage('checkout2') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Kvadrokom/AnsibleDeploy.git'
                sh 'pwd'
                sh 'ls'
                sh 'ls ~/ansible'
                ansiblePlaybook(
                     playbook: 'reminder.yaml',
                     inventory: 'hosts.ini',
                     credentialsId: 'Test_ssh_key_deploy_reminder'
                   )               
                }
            }
    }
    post {
        // failure{
        //     echo 'I will always say Hello only failure'
        //     sh 'rm -rf ~/ansible'
        // }
        // success {
        //     echo 'I will always say Hello only success'
        //     sh 'rm -rf ~/ansible'
        // }
        always {
            echo 'I will always say Hello only again'
            sh 'rm -rf ~/ansible'
            cleanWs()
        }
    }
}
