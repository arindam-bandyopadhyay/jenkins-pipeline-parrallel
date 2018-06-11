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
    # set imagePullPolicy to always
    # and use :latest tag
    # NOTE, this is only for the dev period
    # eventually we will use FIXED versions and imagePullPolicy IfNotPresent
    # https://kubernetes.io/docs/concepts/containers/images/
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

          // doing old scripted way within a new declarative style pipeline
          // snippets below are from the README at https://github.com/jenkinsci/kubernetes-plugin
          // some context between old and new can be found at https://jenkins.io/blog/2017/09/25/declarative-1/

          // TODO
          // archiveArtifacts
          // junit
          // unstash
          // see the list of steps plugins https://jenkins.io/doc/pipeline/steps/

          // TODO
          // come up with a way where the Jenkinsfile does not need to be maintained
          // at least WRT to tests/downstreams jobs.
          // i.e, have the pipeline read test ids from a file
          // we maintain the file, not the test ids.

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