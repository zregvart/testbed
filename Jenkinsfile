pipeline {
  agent any

  triggers {
    issueCommentTrigger('.*test this please.*')
  }

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
            sh "env"
            sh "mkdir -p public"
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
            sh "echo 'hello world 2' > public/index.html"
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
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'public', reportFiles: 'index.html', reportName: 'preview', reportTitles: ''])
      }

      post {
        success {
          script {
            pullRequest.createStatus(status: 'success',
                         context: 'CI Build',
                         description: 'Successfully built, see the website preview by clicking Details',
                         targetUrl: "${env.JOB_URL}/preview")
          }
        }
      }
    }
  }
}
