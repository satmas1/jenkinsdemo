pipeline {
  agent any
  stages {
    stage('Step1') {
      parallel {
        stage('Step1') {
          steps {
            echo 'Hello'
            sh 'echo "Hello"'
          }
        }

        stage('Step11') {
          steps {
            sh 'echo " Build Number $BUILD_NUMBER "'
          }
        }

      }
    }

    stage('Step2') {
      parallel {
        stage('Step2') {
          steps {
            sleep 10
          }
        }

        stage('Step12') {
          steps {
            echo 'Hello'
          }
        }

      }
    }

  }
}