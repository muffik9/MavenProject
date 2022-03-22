pipeline {
  agent any

  tools {
    jdk 'jdk-11'
    maven 'mvn-3.6.3'
  }

  stages {
    stage('Build') {
      steps {
        withMaven(maven : 'mvn-3.6.3') {
          sh "mvn package"
        }
      }
    }

    stage('Test') {
      steps {
        echo "the second stage"
      }
    }
  }
}
