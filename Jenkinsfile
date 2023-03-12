pipeline {

  environment {
    dockerimagename = "rifkiknw/nginx-test02"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/rifkiknw/nginx-test02.git'
      }
    }

    stage('Build image') {  
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          withKubeCredentials(kubectlCredentials: [[credentialsId: 'kubeconfig', serverUrl: 'https://192.168.30.201:6443']]) {
            sh 'kubectl apply -f deploymentservice.yaml'
          }
        }
      }
    }
  }
}
