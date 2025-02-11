pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = 'mariaabadir/weather-app'  // Docker image name
        DOCKERHUB_CREDENTIALS = 'dockerhub'           // Docker Hub credentials ID in Jenkins
        GITHUB_CREDENTIALS = 'github'                 // GitHub credentials ID in Jenkins
        ANSIBLE_PLAYBOOK_PATH = '/home/maria/weatherApp/ansible/playbook.yml'  // Path to your Ansible playbook
        VAGRANT_SSH_KEY_M01 = credentials('ansible_m01')  // First Vagrant machine SSH key
        VAGRANT_SSH_KEY_M02 = credentials('ansible_m02')  // Second Vagrant machine SSH key
        M01_IP = '192.168.56.14'  // IP address of the first Vagrant machine
        M02_IP = '192.168.56.15'  // IP address of the second Vagrant machine
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
                    // Log in to Docker Hub using the stored secret token
                    withCredentials([string(credentialsId: DOCKERHUB_CREDENTIALS, variable: 'DOCKER_PASSWORD')]) {
                        // Use the secret token to log in to Docker Hub
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
                    // Run the Ansible playbook to install Docker on the Vagrant machines and deploy the image
                    withCredentials([file(credentialsId: 'ansible-ssh-private-key-m01', variable: 'SSH_KEY_M01')]) {
                        sh """
                            ansible-playbook -i ${M01_IP}, --private-key ${SSH_KEY_M01} ${ANSIBLE_PLAYBOOK_PATH}
                        """
                    }

                    // Run the playbook for the second machine
                    withCredentials([file(credentialsId: 'ansible-ssh-private-key-m02', variable: 'SSH_KEY_M02')]) {
                        sh """
                            ansible-playbook -i ${M02_IP}, --private-key ${SSH_KEY_M02} ${ANSIBLE_PLAYBOOK_PATH}
                        """
                    }
                }
            }
        }
    }
}
