pipeline {

  environment {
    registry = "lakshmisatya"
    dockerImage = ""
  }

  agent {
       label "slavenode-13"
    }

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/SatyaSri-154/build-deployk8.git'
      }


    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', 'DockerHub' ) {
            dockerImage.push("${env.BUILD_NUMBER}")
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kubeconfig")
        }
      }
    }

  }

}
}


