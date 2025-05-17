pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                git url: 'https://github.com/Asfar-javed/DevOps-TaskManager.git', branch: 'main'
            }
        }

        stage('Clean up CI container') {
            steps {
                sh 'docker rm -f taskmanager-app-ci || true'
            }
        }

        stage('Build & Run CI Container') {
            steps {
                sh 'docker compose -p ecommerce_pipeline -f docker-compose.ci.yml up -d --build'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed. Check logs for details.'
        }
    }
}
