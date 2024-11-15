pipeline {

  environment {
    registry = "docker.io/umamaheswariselvaraj/flask"
    registry_mysql = "docker.io/umamaheswariselvaraj/mysql"
    dockerImage = "latest"
    docker_credentials = 'uma-id'
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/umamaheswariselvarj/Docker-Project.git'
      }
    }

    stage('Build image') {
      steps{
        sh 'docker build -t "docker.io/umamaheswariselvaraj/flask:$BUILD_NUMBER"'
        }
      }
    

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry('https://docker.io', 'docker_credentials') {
            dockerImage.push()
          }
        }
      }
    }

    
   stage('Build mysql image') {
     steps{
       sh 'docker build -t "docker.io/umamaheswariselvaraj/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "docker.io/umamaheswariselvaraj/mysql:$BUILD_NUMBER"'
        }
      }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }
}


