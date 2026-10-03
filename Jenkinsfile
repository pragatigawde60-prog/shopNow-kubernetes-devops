pipeline {
    agent any

    environment {
        BACKEND_IMAGE = 'shopnow-backend:latest'
        FRONTEND_IMAGE = 'shopnow-frontend:latest'
        NAMESPACE = 'shopnow-demo'
        RELEASE_NAME = 'shopnow'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t ${BACKEND_IMAGE} ./backend'
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh 'docker build -t ${FRONTEND_IMAGE} ./frontend'
            }
        }

        stage('Deploy with Helm') {
            steps {
                sh '''
                    helm upgrade --install ${RELEASE_NAME} helm/shopnow \
                    --namespace ${NAMESPACE} \
                    --create-namespace
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'kubectl get pods -n ${NAMESPACE}'
                sh 'kubectl get services -n ${NAMESPACE}'
            }
        }
    }

    post {
        success {
            echo 'ShopNow deployment completed successfully.'
        }

        failure {
            echo 'ShopNow deployment failed. Check the Jenkins console logs.'
        }
    }
}
