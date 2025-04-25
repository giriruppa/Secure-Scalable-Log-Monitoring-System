pipeline {
  agent any
  stages {
    stage('Clone Repo') {
    steps {
        git branch: 'main', url: 'https://github.com/giriruppa/log-monitoring-docker-splunk'
    }
}

    stage('Deploy Splunk') {
      steps {
        dir('splunk-enterprise') {

        }
      }
    }
  }
}
