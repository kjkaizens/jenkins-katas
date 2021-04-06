pipeline {
  agent any
  stages {
    stage('clone down') {
      agent { abel 'master-label'}
      steps {
        stash excludes: '.git', name: 'code'
      }
        
    }
    stage('Parallel exec') {
      parallel {
        stage('Say Hello') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('Build App') {
          agent {
            docker {
              image 'gradle:jdk11'
            }

          }
          steps {
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
          }
        }

      }
    }

  }
}