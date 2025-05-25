pipeline {
    agent any

    environment {
        ANSIBLE_FORCE_COLOR = 'true'
    }

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/Arumrahma20/infra-ansible-jenkins.git'
            }
        }

        stage('Print Working Directory') {
            steps {
                sh 'pwd && ls -lah'
            }
        }

        stage('Test SSH Access') {
            steps {
                sshagent(['ansible-ssh-key']) {
                    sh '''
                        echo "Testing SSH connection..."
                        ssh -vvv -o StrictHostKeyChecking=no rahma1@192.168.100.131 whoami
                    '''
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                sshagent(['ansible-ssh-key']) {
                    sh 'ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i host.ini deploy.yml || exit 1'
                }
            }
        }
    }
}
