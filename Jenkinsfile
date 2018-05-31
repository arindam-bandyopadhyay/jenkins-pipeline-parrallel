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
  stages {
    stage('stage 0') {
      parallel {
        stage('stage1.1') {
          steps {
            container('maven') {
              echo 'from Stage1.1'
            }
          }
        }
        stage('stage 1.2') {
          agent { label 'mypod-stage2' }
          steps {
            container('maven') {
              echo 'from Stage 1.2'
            }
          }
        }
        stage('stage 1.3') {
          steps {
            echo "from stage 1.3" 
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
