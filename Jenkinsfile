pipeline {
    agent any
    
    parameters {
        choice(
            name: 'BOT_NAME',
            choices: ['reminder', 'telebot'],
            description: 'Выберите бота для деплоя'
        )
        choice(
            name: 'STAND_TYPE',
            choices: ['test', 'prod'],
            description: 'Выберите стенд'
        )
    }
    
    environment {
        ANSIBLE_SERVER = '192.168.1.10'
        ANSIBLE_USER = 'rem'
        ANSIBLE_HOME = '/home/rem/Ansible'
        PLAYBOOKS_DIR = "${ANSIBLE_HOME}/playbooks"
        INVENTORY_FILE = "${ANSIBLE_HOME}/inventory/hosts.ini"
    }
    
    stages {
        stage('Подготовка') {
            steps {
                script {
                    // Определяем playbook на основе выбора бота
                    PLAYBOOK_PATH = "${PLAYBOOKS_DIR}/${params.BOT_NAME}.yaml"
                    
                    echo "🚀 Начинаем деплой"
                    echo "🤖 Бот: ${params.BOT_NAME}"
                    echo "🏗️  Стенд: ${params.STAND_TYPE}"
                    echo "📄 Playbook: ${PLAYBOOK_PATH}"
                }
            }
        }
        
        stage('Запуск Ansible') {
            steps {
                echo '🔐 Подключаемся к серверу Ansible...'
                withCredentials([
                    sshUserPrivateKey(
                        credentialsId: 'Ansible-key',
                        usernameVariable: 'USERNAME',
                        keyFileVariable: 'KEYFILE',
                        passphraseVariable: '')
                ]) {
                    sh """
                    #!/bin/bash
                    set -e
                    
                    echo "📡 Устанавливаем SSH соединение..."
                    ssh -i "\$KEYFILE" \
                        -o StrictHostKeyChecking=no \
                        -o BatchMode=yes \
                        "\$USERNAME@${ANSIBLE_SERVER}" << 'EOF'
                    
                    export ANSIBLE_HOST_KEY_CHECKING=False
                    
                    echo "📁 Переходим в директорию Ansible: ${ANSIBLE_HOME}"
                    cd "${ANSIBLE_HOME}"
                    
                    echo "▶️  Запускаем playbook: ${PLAYBOOK_PATH}"
                    ansible-playbook "${PLAYBOOK_PATH}" \
                        -i "${INVENTORY_FILE}" \
                        -e "bot_name=${params.BOT_NAME}" \
                        -e "stand_type=${params.STAND_TYPE}"
                    
                    EOF
                    """
                }
            }
        }
    }
    
    post {
        success {
            echo "✅ Деплой бота '${params.BOT_NAME}' на стенд '${params.STAND_TYPE}' успешно завершен!"
        }
        failure {
            echo "❌ Деплой бота '${params.BOT_NAME}' завершился с ошибкой"
        }
    }
}