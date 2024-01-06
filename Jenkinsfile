pipeline {
  environment {
    dockerimagename = "ashvini34/django-app"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/Ashvini379/Django-Docker-Jenkins-kubernetes.git'
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
          registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploying Django Application container to Kubernetes') {
      steps {
        script {
              sh 'kubectl apply -f $JENKINS_HOME/workspace/deployment.yml'
              sh 'kubectl apply -f $JENKINS_HOME/workspace/service.yml'     
        }   
      }
    }
  }
}