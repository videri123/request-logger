pipeline {
  agent any

  tools {
    maven 'mvn'
  }

  stages {
    stage('Build App') {
      steps {
        sh 'mvn package'
      }
    }
    
    stage('Build Image') {
      steps {
      sh "docker build -t chodavarapusk/request-logger:${env.BUILD_ID} ."
      sh "docker tag chodavarapusk/request-logger:${env.BUILD_ID} chodavarapusk/request-logger:latest"
      }
    }
    
  }

  post {
    always {
      archive 'target/**/*.jar'
    }
    success {
      withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh "docker login -u ${USERNAME} -p ${PASSWORD}"
        sh "docker push chodavarapusk/request-logger:${env.BUILD_ID}"
        sh "docker push chodavarapusk/request-logger:latest"
      }
    }
  }
}
