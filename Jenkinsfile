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
    }
    stages {
        stage('Checkout from Git'){
            steps{
                git branch: 'main', credentialsId: '220d6130-a4a6-4bf0-b696-3785b8ae3321', url: 'https://github.com/Abhin86/Nodejs-Application.git'
            }
        }
        stage("Docker Build"){
            steps{ 
                withCredentials(credentialsId: 'e37672bf-062d-4103-af96-bfe6ad538769', variable: 'secretFile')
                script{
                sh 'sudo -u jenkins docker login -u abhin86 -p $secretFile'
                sh 'sudo -u jenkins docker build -t abhin86/${IMAGE} .'
                    }
                 }
                }
            }
        

        // stage(' Trivy Scan') {
        //     steps {
        //         //  trivy output template 
        //         sh 'sudo curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl > html.tpl'

        //         // Scan all vuln levels
        //         sh 'sudo mkdir -p reports'
        //         sh 'sudo trivy image --format template --template @./html.tpl -o reports/trivy-report.html ${IMAGE}:latest'
        //         publishHTML target : [
        //             allowMissing: true,
        //             alwaysLinkToLastBuild: true,
        //             keepAll: true,
        //             reportDir: 'reports',
        //             reportFiles: 'trivy-report.html',
        //             reportName: 'Trivy Scan',
        //             reportTitles: 'Trivy Scan'
        //         ]

        //         // Scan again and fail on CRITICAL vulns
        //         // sh 'trivy image --ignore-unfixed --vuln-type os,library --exit-code 1 --severity CRITICAL ${IMAGE}:latest '

        //     }
        // }
        stage("Docker Push"){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'e37672bf-062d-4103-af96-bfe6ad538769', toolName: 'docker'){   
                        sh "sudo -u jenkins docker tag ${IMAGE} abhin86/${IMAGE}:${TAG} "
                        sh "sudo -u jenkins docker push abhin86/${IMAGE}:${TAG} "
                        sh "sudo -u jenkins docker tag ${IMAGE} abdulfayis/${IMAGE}:latest "
                        sh "sudo -u jenkins docker push abhin86/${IMAGE}:latest "
                    }
                }
            }
        }
        stage("Docker Clean up "){
            steps{
                 sh 'echo " cleaning Docker Images"'
                 sh 'sudo -u jenkins docker rmi -f \$(sudo docker images -q)'
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
                    sh 'kubectl create ns ${HELM_NAMESPACE} || echo "namespace already created" '
                    sh "helm upgrade --install ${HELM_RELEASE_NAME} ${HELM_CHART_PATH} --set image.tag=${TAG} --namespace=${HELM_NAMESPACE} --wait"
                }
            }
        }
    }

