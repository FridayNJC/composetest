pipeline {
    environment{
        regsitry = "nileshchudasama/composetest"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    agent any

    stages {
        stage('Cloaning our git') {
            steps {
                git 'https://github.com/FridayNJC/composetest.git'
            }
        }
        stage('Building image'){
            steps{
                sh 'docker build -t nileshchudasama/composetest:v2 .'
            }
        }
        stage('Deploy image'){
            steps{
                sh 'docker push nileshchudasama/composetest:v2'
            }
        }
        stage('Deploy'){
            steps{
                script{
                    try{
                        sh 'kubectl apply -f app-deployment.yaml'
                    }
                    catch(error){
                        sh 'kubectl create -f app-deployment.yaml'
                    }
                }
            }
        }
    }
}
