pipeline {
    agent any

    environment {
        ANsible_SERVER = '192.168.1.10'
        PLAYBOOK_PATH = '/home/rem/Ansible/reminder.yaml'
        INVENTORY_FILE = '/home/rem/Ansible/hosts.ini'
    }

    stages {
        stage('Run Ansible Playbook') {
            steps {
                script {
                    def extraVars = ''

                    // Определяем дополнительное значение в зависимости от выбранной ветки
                    switch(params.branch) {
                        case 'reminder':
                            extraVars = '-e STAND_TYPE=prom'
                            break
                        case 'reminder_test':
                            extraVars = '-e STAND_TYPE=test'
                            break
                        default:
                            error("Неверная ветка: ${params.branch}. Поддерживаются только reminder и reminder_test.")
                    }

                    echo 'Starting Ansible Playbook...'
                    withCredentials([
                        sshUserPrivateKey(
                            credentialsId: 'Ansible-key',
                            usernameVariable: 'USERNAME',
                            keyFileVariable: 'KEYFILE',
                            passphraseVariable: '')
                    ]) {
                        sh """
ssh -i \$KEYFILE -o StrictHostKeyChecking=no \$USERNAME@\${ANsible_SERVER} <<EOF
export ANSIBLE_HOST_KEY_CHECKING=False
cd \$(dirname "\${PLAYBOOK_PATH}")
ansible-playbook \${PLAYBOOK_PATH} -i \${INVENTORY_FILE} ${extraVars}
EOF"""
                    }
                }
            }
        }
    }
}