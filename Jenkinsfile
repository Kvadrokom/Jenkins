pipeline {
    agent any
    stages {
        stage('checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Kvadrokom/reminder.git'
                sh 'pwd'
                sh 'ls'
                }
            }
         stage('checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Kvadrokom/Ansible.git'
                sh 'pwd'
                sh 'ls'
                }
            }
        stage('deploy') {
            steps {
                sh 'ansible-playbook -i hosts.ini reminder.yaml'
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
