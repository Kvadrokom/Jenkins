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
            choices: ['test', 'prom'],
            description: 'Выберите стенд'
        )
    }
    
    environment {
        ANSIBLE_SERVER = '192.168.1.10'
        ANSIBLE_USER = 'rem'
        ANSIBLE_HOME = '/home/rem/Deploy'
        PLAYBOOKS_DIR = "${ANSIBLE_HOME}"
        INVENTORY_FILE = "${ANSIBLE_HOME}/hosts.ini"
        ANSIBLE_REPO = "git@github.com:Kvadrokom/Ansible.git"
    }
    
    stages {
        stage('Подготовка') {
            steps {
                script {
                    // Определяем playbook на основе выбора бота
                    PLAYBOOK_PATH = "${PLAYBOOKS_DIR}/${params.BOT_NAME}.yaml"
                    GIT_BRANCH = (params.STAND_TYPE == 'prom') ? 'master' : 'develop'
                    
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
                        keyFileVariable: 'KEYFILE',  // ← Эта переменная!
                        passphraseVariable: ''
                    )
                ]) {
                    sh """
                    ssh -i "\$KEYFILE" \\
                        -o StrictHostKeyChecking=no \\
                        "\$USERNAME@${ANSIBLE_SERVER}" \\
                        "rm -rf ${ANSIBLE_HOME};
                        git clone --branch "${GIT_BRANCH}" --depth 1 "${ANSIBLE_REPO}" "${ANSIBLE_HOME}";\\
                        cd ${ANSIBLE_HOME} && ansible-playbook ${PLAYBOOK_PATH} -i ${INVENTORY_FILE} -e stand_type=${params.STAND_TYPE}"
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