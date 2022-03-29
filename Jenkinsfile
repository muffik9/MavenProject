pipeline {
  agent any

  tools {
    jdk 'jdk-11'
    maven 'mvn-3.6.3'
  }
  environment {
    SONAR_HOME = "${tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'}"
    NEXUS_VERSION = "nexus3"
    NEXUS_PROTOCOL = "http"
    NEXUS_URL = "10.152.183.96:8081"
    NEXUS_REPOSITORY = "nexus_test_repo"
    NEXUS_CREDENTIAl_ID = "Nexus_Access"
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
    stage('Push to Nexus OSS') {
      steps {
        script {
          artifactId = readMavenPom().getArtifactId()
          version = readMavenPom().getVersion()
          groupId = readMavenPom().getGroupId()
          
          echo "*** File: ${artifactId}, ${version}, ${groupId}"
          nexusArtifactUploader (
            nexusVersion: 'nexus3',
            protocol: 'http',
            nexusUrl: '10.152.183.96:8081',
            groupId: "${groupId}",
            version: "${version}",
            repository: 'nexus_test_repo',
            credentialsID: 'Nexus_Access',
            artifacts: [
              [artifactId: "${artifactId}", classifier: '', file: "target/${artifactId}-${version}.jar", type: 'jar'],
              [artifactId: "${artifactId}", classifier: '', file: 'pom.xml', type: 'pom']
            ]
          )
        }
      }
    }
  }
}
