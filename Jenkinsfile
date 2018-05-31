pipeline {
  agent {
    kubernetes {
      //cloud 'kubernetes'
      label 'mypod'
      containerTemplate {
        name 'maven'
        image 'maven:3.3.9-jdk-8-alpine'
        ttyEnabled true
        command 'cat'
      }
    }
  }
  stages {
    stage('stage 0') {
      parallel {
        stage('stage1.1') {
          agent {
            kubernetes {
              label 'mypod-A'
              containerTemplate {
                name 'busybox'
                image 'busybox:latest'
                ttyEnabled true
                command 'cat'
              }
            }
          }
          steps {
            container('busybox') {
              echo 'from Stage1.1'
              sh 'sleep 240'
            }
          }
        }
        stage('stage 1.2') {
          agent {
            kubernetes {
              label 'mypod-B'
              containerTemplate {
                name 'busybox'
                image 'busybox:latest'
                ttyEnabled true
                command 'cat'
              }
            }
          }
          steps {
            container('busybox') {
              echo 'from Stage 1.2'
              sh 'sleep 300'
            }
          }
        }
        stage('stage 1.3') {
          agent {
            kubernetes {
              label 'mypod-C'
              containerTemplate {
                name 'busybox'
                image 'busybox:latest'
                ttyEnabled true
                command 'cat'
              }
            }
          }
          steps {
            container('busybox') {
              echo "from stage 1.3" 
              sh 'sleep 360'
            }
          }
        }
      }
    }
    stage('stage3') {
      steps {
        echo 'from stage3'
      }
    }
  }
}
