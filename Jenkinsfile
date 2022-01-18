pipeline {

  options {
    ansiColor('xterm')
  }

  agent {
    kubernetes {
      inheritFrom 'default'
      yamlFile 'builder.yaml'
    }
  }

  stages {

    stage('Kaniko Build & Push Image') {
      steps {
        container('kaniko') {
          script {
            sh '''
            /kaniko/executor --dockerfile `pwd`/Dockerfile_${registr_repository_name} \
                             --context `pwd` \
                             --destination=alanreynoso/${registr_repository_name}:${BUILD_NUMBER}
            '''
          }
        }
      }
    }

    stage('Deploy App to Kubernetes') {     
      steps {
        container('kubectl') {
          withCredentials([file(credentialsId: 'k3-config', variable: 'KUBECONFIG')]) {
            sh 'uname -a'
            sh 'sed -i "s/<TAG>/${BUILD_NUMBER}/" myweb.yaml'
            sh '#kubectl apply -f myweb.yaml'
          }
        }
      }
    }
  
  }
}