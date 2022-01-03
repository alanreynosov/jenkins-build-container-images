#!groovy

def podLabel = "kaniko-${UUID.randomUUID().toString()}"

pipeline {
    agent {
        kubernetes {
            label podLabel
            defaultContainer 'jnlp'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    jenkins-build: app-build
    some-label: "build-app-${BUILD_NUMBER}"
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:v1.5.1-debug
    imagePullPolicy: IfNotPresent
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
      - name: kaniko-secret
        mountPath: /kaniko/.docker
  volumes:
  - name: kaniko-secret
    projected:
      sources:
      - secret:
          name: dockercreds
          items:
            - key: .dockerconfigjson
              path: config.json
"""
        }
    }

    stages {

        stage('Build with Kaniko') {
          steps {
            container(name: 'kaniko', shell: '/busybox/sh') {
              withEnv(['PATH+EXTRA=/busybox']) {
                sh '''#!/busybox/sh -xe
                  /kaniko/executor \
                    --dockerfile Dockerfile \
                    --context `pwd`/ \
                    --verbosity debug \
                    --insecure \
                    --skip-tls-verify \
                    --destination alanreynoso/myapp:v0.1.0 \
                    --destination alanreynoso/myapp:latest
                '''
              }
            }
          }
        }

    }
}