pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = 'mariaabadir/weather-app'  // Docker image name
        DOCKERHUB_CREDENTIALS = 'dockerhub'           // Docker Hub credentials ID in Jenkins
        GITHUB_CREDENTIALS = 'github'                 // GitHub credentials ID in Jenkins
        ANSIBLE_PLAYBOOK_PATH = 'ansible/playbook.yml'  // Path to your Ansible playbook
        INVENTORY_FILE = 'ansible/inventory'  // Path to your inventory file
    }

    stages {

        stage('Pull Code from GitHub') {
            steps {                 
                git credentialsId: 'github', url: 'https://github.com/MariaAbadir/weatherApp.git', branch: 'main'            
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t ${DOCKER_IMAGE_NAME}:latest .'
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub using stored credentials
                    withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                    }

                    // Push the Docker image to Docker Hub
                    sh 'docker push ${DOCKER_IMAGE_NAME}:latest'
                }
            }
        }

         stage('Run Ansible Playbook') {
            steps {
                script {
                    // Retrieve both SSH keys
                    withCredentials([
                        sshUserPrivateKey(credentialsId: 'ansible_m01', keyFileVariable: 'SSH_KEY_M01', usernameVariable: 'SSH_USER_M01'),
                        sshUserPrivateKey(credentialsId: 'ansible_m02', keyFileVariable: 'SSH_KEY_M02', usernameVariable: 'SSH_USER_M02')
                    ]) {
                       sh """
                           ANSIBLE_SSH_ARGS="-o StrictHostKeyChecking=no -i ${SSH_KEY_M01} -i ${SSH_KEY_M02}" \
                           ansible-playbook -i ${INVENTORY_FILE} ${ANSIBLE_PLAYBOOK_PATH}
                       """
                    }
                }
            }
        }
    }
}
