pipeline {
  environment {
    registry = "192.168.1.59:5000/justme/myweb"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        echo 'getting git repo ...'
        hostname
        git 'https://github.com/erick66sap/kube2.git'
      }
    }
    stage('Build image') {
      steps{
        echo 'Building image...'
        hostname
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Push Image') {
      steps{
        hostname
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Deploy App') {
      steps {
        hostname
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }
  }
}
