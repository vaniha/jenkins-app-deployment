pipeline {

  environment {
    dockerimagename = "vaniha/project-deployment"
    dockerImage = ""
  }

  agent { label 'dind-agent'}

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/vaniha/jenkins-app-deployment.git'
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
          kubernetesDeploy(configs: "deployment.yaml", kubeconfigId: "kubernetes");
          kubernetesDeploy(configs: "service.yaml", kubeconfigId: "kubernetes");
        }
        }
    }

  }

}
