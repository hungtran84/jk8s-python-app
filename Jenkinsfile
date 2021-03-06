pipeline {
  agent {
    kubernetes {
      label 'runner'
      idleMinutes 5
      yamlFile 'agent.yaml'  
      defaultContainer 'python'  
    }
  }

  environment {
      DOCKERHUB_PW = credentials('dockerhub_pw')
  }
  
  stages {
      stage('Python runner testing') {
          steps {
              sh 'python -V'
          }
      }

      stage('Build and push Docker Image') {
          steps {
            container('docker') {  
              sh "docker build -t kanchimo/python-simple-app:$BUILD_NUMBER -t kanchimo/python-simple-app:latest ."
              sh "docker login -u kanchimo -p $DOCKERHUB_PW"
              sh "docker push kanchimo/python-simple-app:$BUILD_NUMBER"
              sh "docker push kanchimo/python-simple-app:latest"
            }
          }
      }

      stage('Deploy to dev') {
          steps {
            container('kubectl') {  
              sh "version"
            }
          }
      }
  }
}