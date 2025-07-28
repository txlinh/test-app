pipeline {
  agent any

  environment {
    IMAGE_NAME = 'trinhxuanlinh/nodejs-app'
  }

  stages {
    stage('Clone') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          docker.build("${IMAGE_NAME}:latest")
        }
      }
    }

    stage('Push to DockerHub') {
      steps {
        withCredentials([string(credentialsId: 'dockerhub-password', variable: 'DOCKER_PASS')]) {
          sh """
            echo $DOCKER_PASS | docker login -u your-dockerhub-username --password-stdin
            docker push ${IMAGE_NAME}:latest
          """
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f deployment.yml'
        sh 'kubectl apply -f service.yml'
      }
    }
  }
}
