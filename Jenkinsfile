pipeline {
  agent none
  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/srdrcn/playjenkins.git'
      }
    }

    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }

      }
    }

    stage('Push Image') {
      steps {
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }

      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }

      }
    }

  }
  environment {
    registry = 'http://192.168.1.10:5000/justme/myweb'
    registryCredential = 'dockerid'
    dockerImage = ''
  }
}