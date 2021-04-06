pipeline {
  agent any
  stages {
    stage('clone down') {
      agent { label 'master-label'}
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
          options{
            skipDefaultCheckout(true)
          }
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

        stage('test app') {
          agent {
            docker {
              image 'gradle:jdk11'
            }

          }
          options {
            skipDefaultCheckout()
          }
          steps {
            unstash 'code'
            sh 'ci/unit-test-app.sh'
            junit 'app/build/test-results/test/TEST-*.xml'
          }
        }
      }
    }

  }
}