pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  name: kaniko
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:latest
    args:
    - "--context=git://github.com/scriptcamp/kubernetes-kaniko"
    - "--destination=alanreynoso/kaniko-demo-image:1.0"
    volumeMounts:
    - name: kaniko-secret
      mountPath: /kaniko/.docker
  restartPolicy: Never
  volumes:
  - name: kaniko-secret
    secret:
      secretName: dockercred
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
          sh '''#!/busybox/sh
            echo "FROM jenkins/inbound-agent:latest" > Dockerfile
            /kaniko/executor --context `pwd` --destination <docker-username>/hello-kaniko:latest 
          '''
        }
      }
    }
  }
}