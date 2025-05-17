pipeline {
    agent any

    environment {
        MONGO_URI = credentials('MONGO_URI')
        PORT = credentials('PORT')
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git url: 'https://github.com/Asfar-javed/DevOps-TaskManager.git', branch: 'main'
            }
        }

        stage('Create .env file') {
            steps {
                script {
                    sh '''
                        cat > .env << EOF
                        MONGO_URI = credentials('MONGO_URI')
                        PORT = credentials('PORT')
                        EOF
                    '''
                }
            }
        }

        stage('Clean up CI container') {
            steps {
                // Remove only the CI container, not production (Part-I)
                sh '''
                    docker rm -f taskmanager-app-ci || true
                '''
            }
        }

        stage('Build & Run CI Container') {
            steps {
                sh 'docker-compose -p ecommerce_pipeline -f docker-compose.ci.yml up -d --build'
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
