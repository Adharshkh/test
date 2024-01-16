pipeline {
agent any
    environment {
        IMAGE = 'nodejs'
        TAG = "${BUILD_NUMBER}"
        PROJECT_ID = 'kubernetes2-410610'
        CLUSTER_NAME = 'gkecluster'
        LOCATION = 'us-central1-a'
        CREDENTIALS_ID = '220d6130-a4a6-4bf0-b696-3785b8ae3321'
        HELM_CHART_PATH = 'nodels-application/'
        HELM_RELEASE_NAME = 'nodejs'
        HELM_NAMESPACE = 'nodejs'
        DOCKER_HUB_CREDENTIALS = credentials('e37672bf-062d-4103-af96-bfe6ad538769')
        REPOSITORY_NAME = 'abhin86'

    }
    stages {
        stage('Checkout from Git'){
            steps{
                git branch: 'main', credentialsId: '220d6130-a4a6-4bf0-b696-3785b8ae3321', url: 'https://github.com/Abhin86/Nodejs-Application.git'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Use Docker Hub credentials from Jenkins credentials
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'e37672bf-062d-4103-af96-bfe6ad538769', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]) {
                        
                        // Login to Docker with credentials and --password-stdin
                        sh "echo \${DOCKER_PASSWORD} | sudo -u jenkins docker login -u \${DOCKER_USERNAME} --password-stdin "
                        
                        // Build the Docker image
                        sh "sudo -u jenkins docker build -t \${REPOSITORY_NAME}/\${REPOSITORY_NAME}:\${TAG} ."
                        sh "sudo -u jenkins docker tag \${REPOSITORY_NAME}/\${REPOSITORY_NAME}:\${TAG} \${REPOSITORY_NAME}/\${REPOSITORY_NAME}:\${TAG}"
                        sh "sudo -u jenkins docker push \${REPOSITORY_NAME}/\${REPOSITORY_NAME}:\${TAG}"
                        sh 'echo " cleaning Docker Images"'
                        sh 'sudo -u jenkins docker rmi -f \$(sudo -u jenkins docker images -q)'
                    }
                }
            }
        }
             stage("Helm install "){
            steps{
                 sh "curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3"
                 sh "chmod 700 get_helm.sh"
                 sh "./get_helm.sh"
            }
        }
            stage('GKE Authentication') {
            steps{
                    sh "gcloud container clusters get-credentials ${CLUSTER_NAME} --zone ${LOCATION} --project ${PROJECT_ID}"
                
            }
        }
         stage('Deploy Helm Chart') {
            steps {
                script {
                    sh 'gcloud components install kubectl'
                    sh "helm upgrade --install node-app ./nodejs-application -n nodejs --create-namespace"                }
            }
        }
    }

}
        
       

       
    

