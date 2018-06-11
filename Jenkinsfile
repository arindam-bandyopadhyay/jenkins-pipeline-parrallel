pipeline {
  agent {
    kubernetes {
      label 'mypod'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: glassfish-ci
    image: arindamb/glassfish-ci
    command:
    - cat
    tty: true
"""
    }
  }
  environment {
    S1AS_HOME = "$WORKSPACE/glassfish5/glassfish"
    APS_HOME = "$WORKSPACE/appserver/tests/appserv-tests"
    TEST_RUN_LOG = "$WORKSPACE/tests-run.log"
    MAVEN_REPO_LOCAL = "$WORKSPACE/repository"
  }
  stages {
    stage('glassfish-build') {
      agent {
        kubernetes {
          label 'mypod-A'
        }
      }
      steps {
        container('glassfish-ci') {
          echo 'from non parallel stage'
        }
      }
    }
    stage('glassfish-functional-tests') {
      steps {
        script {
          def tests = [:]
          for (i = 0; i <3; i++) {
            tests["${i}"] = {
              def label = "mypod-A"
              podTemplate(label: label) {
                  node(label) {
                      stage('Run shell') {
                        container('glassfish-ci') {
                          sh 'echo hello world'
                          sh 'for i in `seq 120` ; do echo "i=${i}" ; sleep 1 ; done'
                        }
                      }
                  }
              }
            }
          }
          parallel tests
        }
      }
    }
  }
}