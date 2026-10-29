pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/atullahase3049/website.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t atul03030304/devops-website:latest .'
            }
        }

        stage('Docker Hub Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push atul03030304/devops-website:latest
                        docker logout
                    '''
                }
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                sh '''
                    ssh -i /home/ubuntu/Project.pem \
                        -o StrictHostKeyChecking=no \
                        ubuntu@172.31.9.162 \
                        "KUBECONFIG=/home/ubuntu/.kube/config kubectl set image deployment/nginx-app nginx=atul03030304/devops-website:latest && KUBECONFIG=/home/ubuntu/.kube/config kubectl rollout status deployment/nginx-app"
                '''
            }
        }
    }
}
