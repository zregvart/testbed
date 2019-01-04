pipeline {
  agent any

  stages {
    stage('Test') {
      steps {
        script {
          pullRequest.addLabel('hello')
        }
      }
    }
  }
}
