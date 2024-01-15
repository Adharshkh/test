pipeline {
  agent any
  environment {
    PROJECT = "kubernetes2-410610"
    APP_NAME = "nodejs-app"
    REPO_NAME = "nodejs2"
    REPO_LOCATION = "us-central1"
    IMAGE_NAME = "${REPO_LOCATION}-docker.pkg.dev/${PROJECT}/${REPO_NAME}/${APP_NAME}"
  }

      stages {
        stage('Git checkout') {
            steps {
                git branch: 'main', credentialsId: '220d6130-a4a6-4bf0-b696-3785b8ae3321', url: 'https://github.com/Abhin86/Nodejs-Application'
            }
        }
  // stages {
  //   stage('Pull Git'){
  //     when { expression { true } }
  //     steps{
  //       checkout scm
  //     }
  //   }
  // }
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
