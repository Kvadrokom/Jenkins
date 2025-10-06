pipeline {
    agent any
    environment {
        ANsible_SERVER = '192.168.1.10'
        PLAYBOOK_PATH = '/home/rem/Ansible/reminder/tasks/main.yaml'
        INVENTORY_FILE = '/home/rem/Ansible/hosts.ini'
    }
    stages {
        stage('Run Ansible Playbook') {
            steps {
                script {
                    echo 'Starting Ansible Playbook...'
                    sh """
                        echo $USER
                        ssh -o StrictHostKeyChecking=no \$USER@\${ANsible_SERVER} <<EOF
                            export ANSIBLE_HOST_KEY_CHECKING=False
                            cd \$(dirname "\${PLAYBOOK_PATH}")
                            ansible-playbook \${PLAYBOOK_PATH} -i \${INVENTORY_FILE}
                        EOF
                    """
                }
            }
        }
    }
}