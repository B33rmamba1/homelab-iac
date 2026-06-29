pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
        ANSIBLE_PRIVATE_KEY = '/var/jenkins_home/.ssh/id_ed25519_carefortress'
        ANSIBLE_SSH_ARGS = '-o StrictHostKeyChecking=no -o IdentitiesOnly=yes'
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
                sh 'ansible -i ${INVENTORY} -m ping all --private-key ${ANSIBLE_PRIVATE_KEY} --ssh-extra-args "${ANSIBLE_SSH_ARGS}"'
            }
        }

        stage('Run Provision Playbook') {
            steps {
                sh 'ansible-playbook -i ${INVENTORY} ${PLAYBOOKS}/provision.yml --private-key ${ANSIBLE_PRIVATE_KEY} --ssh-extra-args "${ANSIBLE_SSH_ARGS}"'
            }
        }

        stage('Run Patch Playbook') {
            steps {
                sh 'ansible-playbook -i ${INVENTORY} ${PLAYBOOKS}/patch.yml --private-key ${ANSIBLE_PRIVATE_KEY} --ssh-extra-args "${ANSIBLE_SSH_ARGS}"'
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
