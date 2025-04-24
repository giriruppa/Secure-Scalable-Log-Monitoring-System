pipeline {
  agent any
  stages {
    stage('Clone Repo') {
      steps {
        git 'https://github.com/giriruppa/log-monitoring-docker-splunk'
      }
    }
    stage('Deploy Splunk') {
      steps {
        dir('splunk-enterprise') {
          sh 'docker-compose down'
          sh 'docker-compose up -d'
        }
      }
    }
  }
}
