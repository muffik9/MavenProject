pipeline {
  agent any

  tools {
    jdk 'jdk-11'
    maven 'mvn-3.6.3'
  }
  environment {
    SONAR_HOME = "${tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'}"
  }
  stages {
    stage('Build') {
      steps {
        withMaven(maven : 'mvn-3.6.3') {
          sh "mvn clean install package"
        }
      }
    }

    stage('SonarQube Analysis') {
      steps {
	script {
	  scannerHome = tool 'sonar-scanner'
	  }
	withSonarQubeEnv('sonar') {
	sh """ mvn sonar:sonar -Dsonar.projectKey=pipeline -Dsonar.host.url=http://10.152.183.166:9000 -Dsonar.login=a992b80128226407753cf7635aa5173273f65852 """
        }
      }
    }
  }
}
