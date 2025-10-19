pipeline {
    agent any

    environment {
        ANSIBLE_INVENTORY = "hosts"
        PLAYBOOK_FILE = "playbook.yml"
    }

    stages {

        stage('📦 Checkout Code') {
            steps {
                echo "Checking out the repository..."
                checkout scm
            }
        }

        stage('⚙️ Install Ansible') {
            steps {
                echo "Installing Ansible..."
                sh '''
                    if ! command -v ansible >/dev/null 2>&1; then
                        sudo apt update -y
                        sudo apt install -y ansible
                    else
                        echo "✅ Ansible already installed"
                    fi
                '''
            }
        }

        stage('🧠 Validate Playbook Syntax') {
            steps {
                echo "Validating Ansible syntax..."
                sh "ansible-playbook --syntax-check ${PLAYBOOK_FILE}"
            }
        }

        stage('🚀 Run Ansible Playbook') {
            steps {
                echo "Running Ansible Playbook on target servers..."
                sh """
                    ansible-playbook -i ${ANSIBLE_INVENTORY} ${PLAYBOOK_FILE} \
                    --ssh-extra-args='-o StrictHostKeyChecking=no'
                """
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful! Nginx configured on all targets."
        }
        failure {
            echo "❌ Deployment failed! Check Ansible logs above."
        }
        always {
            echo "📜 Pipeline finished — check console output for full logs."
        }
    }
}
