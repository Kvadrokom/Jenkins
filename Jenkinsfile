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
ansible-playbook \${PLAYBOOK_PATH} -i \${INVENTORY_FILE} ${params.STAND_TYPE}
EOF"""
                    }
                }
            }
        }
    }