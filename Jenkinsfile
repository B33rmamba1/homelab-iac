pipeline {
    agent any

    environment {
        ANSIBLE_CONFIG = '/var/jenkins_home/homelab-iac/ansible.cfg'
        INVENTORY = '/var/jenkins_home/homelab-iac/inventory/carefortress.ini'
        PLAYBOOKS = '/var/jenkins_home/homelab-iac/playbooks'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Pipeline started - using committed playbooks'
            }
        }

        stage('Verify Connectivity') {
            steps {
                sh 'ansible -i ${INVENTORY} -m ping all'
            }
        }

        stage('Run Provision Playbook') {
            steps {
                sh 'ansible-playbook -i ${INVENTORY} ${PLAYBOOKS}/provision.yml'
            }
        }

        stage('Run Patch Playbook') {
            steps {
                sh 'ansible-playbook -i ${INVENTORY} ${PLAYBOOKS}/patch.yml'
            }
        }
    }

    post {
        success {
            echo 'All playbooks completed successfully.'
        }
        failure {
            echo 'Pipeline failed - check Ansible output above.'
        }
    }
}
