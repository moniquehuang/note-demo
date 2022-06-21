def githubUser = "moniquehuang"
def projectName = "note-demo"
def version = "0.0.1"
def dockerImageTag = "${githubUser}/${projectName}"

pipeline {
  agent any

  stages {
    stage('Test') {
      steps {
        echo "Testing..."
      }
    }

    stage('Build') {
      steps {
        echo "Building..."
        sh "docker build -t ${dockerImageTag} ."
      }
    }

    stage('Deploy Container To Openshift') {
      steps {
        echo "Deploying..."
        sh "oc login https://localhost:8443 --username admin --password admin --insecure-skip-tls-verify=true"
        sh "oc project ${projectName} || oc new-project ${projectName}"
        sh "oc delete all --selector app=${projectName} || echo 'Unable to delete all previous openshift resources'"
        sh "oc new-app ${dockerImageTag} -l version=${version}"
        sh "oc expose svc/${projectName}"
      }
    }
  }
}