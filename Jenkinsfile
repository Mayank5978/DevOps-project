pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps { git 'https://github.com/YOUR_USERNAME/devops-project.git' }
        }
        stage('Build Docker') {
            steps { sh 'docker build -t YOUR_DOCKERHUB_USERNAME/webapp:${BUILD_NUMBER} .' }
        }
        stage('Push to Docker Hub') {
            steps { 
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh 'docker push YOUR_DOCKERHUB_USERNAME/webapp:${BUILD_NUMBER}'
                    }
                }
            }
        }
        stage('Terraform Apply') {
            steps { sh 'cd terraform && terraform init && terraform apply -auto-approve' }
        }
        stage('Ansible Deploy') {
            steps { sh 'ansible-playbook -i ansible/inventory.ini ansible/playbook.yml' }
        }
    }
}
