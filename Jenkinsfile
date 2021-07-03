pipeline {
  agent any
  environment {
    DOCKERHUB_USER = "kubechi"
    BUILD_HOST = "root@192.168.88.101"
    BUILD_TIMESTAMP = sh(script: "date +%Y%m%d-%H%M%S", returnStdout: true).trim()
  }
  stages {
    stage('Pre Check') {
      steps {
        sh "test -f ~/.docker/config.json"
        sh "cat ~/.docker/config.json | grep docker.io"
      }
    }
    stage('Build') {
      steps {
        sh "cat docker-compose.yml"
        sh "docker-compose -H ssh://${BUILD_HOST} -f docker-compose.yml down"
        sh "docker -H ssh://${BUILD_HOST} volume prune -f"
        sh "docker-compose -H ssh://${BUILD_HOST} -f docker-compose.yml build"
        sh "docker-compose -H ssh://${BUILD_HOST} -f docker-compose.yml up -d"
        sh "docker-compose -H ssh://${BUILD_HOST} -f docker-compose.yml ps"
      }
    }
  }
}