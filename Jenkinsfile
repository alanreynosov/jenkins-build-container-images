pipeline {
    agent any

    stages {
        stage{
            steps('test'){
             sh 'kubectl get all --all-namespaces -o wide'   
            }
        }
    }

}        