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
    stage('stage1') {
      parallel {
        stage('stage1') {
          steps {
            container('maven') {
              echo 'from Stage1'
            }
          }
        }
        stage('stage2') {
          steps {
            container('maven') {
              echo 'from Stage 3'
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
