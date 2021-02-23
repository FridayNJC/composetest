pipeline {
    environment{
        regsitry = "nileshchudasama/composetest"
        registryCredential = 'nileshchudasama'
        dockerImage = ''
    }
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/FridayNJC/composetest.git']]])
            }
        }
        stage('Build'){
            steps{
                git 'https://github.com/FridayNJC/composetest.git'
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy Image'){
            steps{
                script{
                    docker.withRegistry('', registryCredential){
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Test'){
            steps{
                sh 'curl http://localhost:5000'
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
