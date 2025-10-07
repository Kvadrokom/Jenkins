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
                    echo 'Starting Ansible Playbook...'
                    withCredentials([
                        sshUserPrivateKey(credentialsId: 'Ansible-key', // ID секрета в Jenkins
                                          usernameVariable: 'USERNAME', // Переменная для имени пользователя
                                          keyFileVariable: 'KEYFILE',   // Переменная для SSH-ключа
                                          passphraseVariable: '')        // Переменная для пароля (оставляем пустой, если нет пароля)
                    ]) {
                        // Внутри блока используем переменные USERNAME и KEYFILE
                        sh """
ssh -i \$KEYFILE -o StrictHostKeyChecking=no \$USERNAME@\${ANsible_SERVER} <<EOF
 export ANSIBLE_HOST_KEY_CHECKING=False
cd \$(dirname "\${PLAYBOOK_PATH}")
ansible-playbook \${PLAYBOOK_PATH} -i \${INVENTORY_FILE}
EOF"""
                    }
                }
            }
        }
    }
}