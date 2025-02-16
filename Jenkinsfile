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
    NEXUS_REPOSITORY = "nexus_test"
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
    stage('maven gets info') {
      steps {
        script {
          pom = readMavenPom file: "pom.xml"
          filesByGlob = findFiles(glob: "server/target/*.${pom.packaging}")
          echo "This is echo for variable ${filesByGlob}"
          echo "This is to see if there is anything here:"
          ls server/target
          ls webapp/target
          testinglocation = pwd
          echo "$testinglocation"
          echo "spacer"
          }
        }
      }
    stage('Push to Nexus OSS') {
      steps {
        script {
          pom = readMavenPom file: "pom.xml"
          filesByGlob = findFiles(glob: "target/*.${pom.packaging}")
          version = readMavenPom().getVersion()
          groupId = readMavenPom().getGroupId()
          
          echo "*** File: ${artifactId}, ${version}, ${groupId}, ${pom.packaging}"
          nexusArtifactUploader (
            nexusVersion: 'nexus3',
            protocol: 'http',
            nexusUrl: '10.152.183.96:8081',
            groupId: "${groupId}",
            version: "${version}",
            repository: 'nexus_test',
            credentialsId: 'Nexus_Access',
            artifacts: [
              [artifactId: pom.artifactId,
              classifier: '',
              file: artifactPath,
              type: pom.packaging],
              [artifactId: pom.artifactId,
              classifier: '',
              file: "pom.xml",
              type: "pom"]
            ]
          )
        }
      }
    }
  }
}
