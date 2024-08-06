pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'surenderj/py-flask:latest'
        KUBERNETES_DEPLOYMENT = 'flask-app'
        KUBECONFIG = '/root/.kube/config'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo/flask-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry(credentialsId: 'docker-registry-credentials', url: 'https://index.docker.io/v1/') {
                    script {
                        docker.image("${DOCKER_IMAGE}").push('latest')
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig(credentialsId: 'kubeconfig-credentials') {
                    script {
                        sh 'kubectl apply -f k8s-deployment.yaml'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
