pipeline {
    agent any
    
    environment {
        PROJECT_ID = 'kubernetes2-410610'
        REGISTRY = 'gcr.io'
        APP_NAME = 'nodejs2'
        HELM_RELEASE_NAME = 'nodejs2'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from GitHub
                checkout scm
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                // Build Docker image
                script {
                    docker.build("us-central1-docker.pkg.dev/kubernetes2-410610/nodejs2:${env.BUILD_NUMBER}" , '-f Dockerfile .')
                }

                // Push Docker image to Google Artifact Registry
                script {
                    docker.withRegistry('https://gcr.io', 'kubernetes2-410610') {
                        docker.image("${REGISTRY}/${PROJECT_ID}/${APP_NAME}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }

        stage('Deploy Helm Chart') {
            steps {
                // Authenticate with Google Cloud
                script {
                    withCredentials([file(credentialsId: 'e8784de2-a680-431f-b817-a46befa6ea70', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                        sh 'gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS'
                    }
                }

                // Deploy Helm chart
                script {
                    sh "helm upgrade --install ${HELM_RELEASE_NAME} -n nodejs --create-namespace nodejs-application/"
                }
            }
        }
    }

    post {
        success {
            echo 'Build, push, and deployment successful!'
        }
    }
}
