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
    stage('stage0') {
      parallel {
        stage('stage1') {
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
              label 'mypod-stage2'
              containerTemplate {
                name 'maven'
                image 'maven:3.5.2-jdk-8-alpine'
                ttyEnabled true
                command 'cat'
              }
            }
          }
          steps {
            container('maven') {
              echo 'from Stage 2'
              sh 'sleep 120'
            }
          }
        }
      }
    }
    stage('stage3') {
      steps {
        echo 'from stage3'
        sh 'sleep 300'
      }
    }
  }
}
