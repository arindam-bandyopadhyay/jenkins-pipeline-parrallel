pipeline {
  agent none
  stages {
    stage('stage0') {
      parallel {
        stage('stage1') {
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
          steps {
            container('maven') {
              echo 'from Stage1'
            }
          }
        }
        stage('stage2') {
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
          steps {
            container('maven') {
              echo 'from Stage 3'
            }
          }
        }
      }
    }
    stage('stage3') {
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
      steps {
        echo 'from stage3'
        sh 'sleep 300'
      }
    }
  }
}
