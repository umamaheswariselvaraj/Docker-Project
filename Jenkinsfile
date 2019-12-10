pipeline {

  environment {
    registry = "172.31.1.107:5000/mgsgoms/flask"
    registry_mysql = "172.31.1.107:5000/mgsgoms/mysql"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/mgsgoms/Docker-Project.git'
      }
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
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('current directory') {
      steps{
        sh 'docker build -t 172.31.1.107:5000/mgsgoms/mysql:$BUILD_NUMBER" /home/jenkins/agent/workspace/Docker-Projects_master/mysql/'
        sh 'docker push "172.31.1.107:5000/mgsgoms/mysql:$BUILD_NUMBER"'
        }
      }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}