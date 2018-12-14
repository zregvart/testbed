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
            def comment_text = "See the preview at ${JOB_URL}/preview"
            def found = false
            for (comment in pullRequest.comments) {
              if (comment.user == 'camelesi' && comment.body == comment_text) {
                found = true
                break;
              }
            }

            if (!found) {
              pullRequest.comment(comment_text)
            }
          }
        }
      }
    }
  }

  post {
    failure {
      script {
        if (env.CHANGE_ID) {
          pullRequest.addLabel('ci:failed')
        }
      }
    }
  }
}
