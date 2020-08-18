pipeline {
    environment {
        registry = "vijaynagothi/simplilearn-devops-certification"
        registryCredential = 'dockerhub-creds'
    }
    agent any

    stages {
        stage('SCM checkout') {
            steps{
                echo 'Git checkout'
                git 'https://github.com/vijaynagothi/dockerize-jenkins-pipeline.git'
            }
        }
        
        /*stage('Build Docker image') {
            steps{
                echo 'Building Image from Dckerfile'
                sh "docker build -t vijaynagothi/simplilearn-devops-certification:${env.BUILD_ID} ."
                sh "docker images"
            }
        }
        
        stage('Puch Docker image') {
            steps{
                echo 'Pushing Image to DockerHub'
                withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                    sh "echo ${dockerhubpwd} | docker login -u vijaynagothi --password-stdin"
                }
                 sh "docker push vijaynagothi/simplilearn-devops-certification:${env.BUILD_ID}"
            }
        }*/
        stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy Image') {
          steps{
            script {
              docker.withRegistry( '', registryCredential ) {
                dockerImage.push()
              }
            }
          }
        }
        stage('Remove Unused docker image') {
          steps{
            sh "docker rmi $registry:$BUILD_NUMBER"
          }
        }
    }
}

