pipeline {
    agent any

    stages {
        stage('test'){
            steps{
             sh 'kubectl get all --all-namespaces -o wide'   
            }
        }
    }

}        