pipeline {
  environment {
    PROJECT_ID = 'beaming-force-358817'
    LOCATION = 'us-central1'
    registry = "gcr.io/beaming-force-358817/gke-gcr"
    CREDENTIALS_ID = 'gke'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/anishnath/mkdocs.git'
      }
    }
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
          docker.withRegistry( '', CREDENTIALS_ID ) {
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
