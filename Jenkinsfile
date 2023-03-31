pipeline {
  agent any
  environment {
    DOCKER_HOST = "unix:///var/run/docker.sock"
    IMAGE_NAME = 'rzlinux0/proj3:${BUILD_NUMBER}'
    awsPath = "/var/jenkins_home/.aws"
  }
  stages {
    stage('Checkout') {
      steps {
        cleanWs()
        sh 'git clone -b main https://github.com/XXcage/Proj3.git'
      }
    }
    stage('.aws config and credentials') {
      steps {
        script {
          //echo "Image name is: ${imageName}"
          //sh "pwd"
          //sh "ls -la"
          //def awsPath = "/var/jenkins_home/.aws"
          //sh "cp -r ${awsPath} Proj3/"
          sh "cp -r ${env.awsPath} Proj3/"
        }
      }
    }
    stage('Build Docker Image') {
      steps {
        dir('Proj3') {
          script {
            // sh "pwd"
            // sh "ls -la"
            //def buildNumber = env.BUILD_NUMBER ?: 'unknown'
            //def imageName = "rzlinux0/proj3:${buildNumber}"
            //echo "Image name is: ${imageName}"
            //sh "docker build -t ${imageName} ."
            sh "docker build -t ${env.IMAGE_NAME} ."
            //env.IMAGE_NAME = imageName
          }      
        }
      }
    }
    stage('echo ENV Image name') {
      steps {
        script {
          sh "echo ${env.IMAGE_NAME}"
          // deploy the image to staging
        }
      }
    }
    stage('DockerHub Login') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'DockerHubID', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
          sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
        }
      }
    }
    stage('Push Docker Image') {
      steps {
        sh "docker push ${env.IMAGE_NAME}"
      }
    }
    stage('Run Docker Image') {
      steps {
        script {
          sh "docker run --rm ${env.IMAGE_NAME}"
        }
      }
    }    
  }
}
