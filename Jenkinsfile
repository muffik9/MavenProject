pipeline {
  agent any

  tools {
    jdk 'jdk-11'
    maven 'mvn-3.6.3'
    sonarqube 'sonarqube-4.7.0.2747'
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
        withMaven(maven : 'mvn-3.6.3') {
	  sh "mvn sonar:sonar -Dsonar.login=myAuthenticationToken"
        }
      }
    }
  }
}
