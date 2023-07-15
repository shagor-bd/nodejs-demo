pipeline {
    agent any
    environment {
    DOCKERHUB_CREDENTIALS = credentials('docker-hub-shagor')
    }
    stages { 
        stage('SCM Checkout') {
            steps{
            git 'https://github.com/shagor-bd/nodejs-demo.git'
            }
        }

        stage('Build docker image') {
            steps {  
                sh 'docker build -t azizctg08/nodeapp:$BUILD_NUMBER .'
            }
        }
        stage('login to dockerhub') {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh 'docker push azizctg08/nodeapp:$BUILD_NUMBER'
            }
        }
        stage('Deploying App to Kubernetes') {
            steps {
                script {
                    kubernetesDeploy(configs: "deploymentservice.yml")
                }
            }
        }

}
post {
        always {
            sh 'docker logout'
        }
    }
}
