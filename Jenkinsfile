pipeline {
    agent any

    stages {
        stage{
            steps{
             sh 'kubectl get all --all-namespaces -o wide'   
            }
        }
    }

}