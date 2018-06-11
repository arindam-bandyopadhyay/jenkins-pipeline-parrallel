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
              stage('blabla${i}'){
                steps {
                  container('test-${i}') {
                    echo 'from non parallel stage'
                  }
                }
              }
            }
          }
          echo "${tests}"
          parallel tests
        }
      }
    }
  }
}
