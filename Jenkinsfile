pipeline {
  options {
    buildDiscarder(
        logRotator(artifactNumToKeepStr: '5', numToKeepStr: '10')
        )
  }

  stages {
    stage('Build') {
      stages {
        stage('Theme') {
          steps {
            sh "mkdir public"
            sh "touch public/theme"
          }
        }

        stage('Documentation') {
          steps {
            sh "touch public/docs"
          }
        }

        stage('Website') {
          steps {
            sh "echo hello world > public/index.html"
          }
        }
      }
    }

    stage('Preview') {
      when {
        not {
          branch 'master'
        }
      }

      steps {
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'public', reportFiles: 'index.html', reportName: 'Preview', reportTitles: ''])
      }
    }
  }
}
