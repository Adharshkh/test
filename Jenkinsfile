pipeline {
  environment {
    PROJECT = "kubernetes2-410610"
    APP_NAME = "nodejs-app"
    REPO_NAME = "nodejs2"
    REPO_LOCATION = "us-central1"
    IMAGE_NAME = "${REPO_LOCATION}-docker.pkg.dev/${PROJECT}/${REPO_NAME}/${APP_NAME}"
  }

  stages {
    stage('Pull Git'){
      when { expression { true } }
      steps{
        checkout scm
      }
    }

//     stage('Build docker image') {
//     when { expression { true } }
//       steps{
//         container('docker'){
//           dir('Backend Net/MyDotnet') {
//             echo 'Build docker image Start'
//             sh 'pwd'
//             sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
//             withCredentials([file(credentialsId: "${PROJECT}_artifacts", variable: 'GCR_CRED')]){
//               sh 'cat "${GCR_CRED}" | docker login -u _json_key_base64 --password-stdin https://"${REPO_LOCATION}"-docker.pkg.dev'
//               sh 'docker push ${IMAGE_NAME}:${IMAGE_TAG}'
//               sh 'docker logout https://"${REPO_LOCATION}"-docker.pkg.dev'
//             }
//             sh 'docker rmi ${IMAGE_NAME}:${IMAGE_TAG}'
//             echo 'Build docker image Finish'
//           }
//         }
//       }
//     }
//   }
}
