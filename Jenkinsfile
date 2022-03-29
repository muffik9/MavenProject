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
          pom = readMavenPom file: "pom.xml";
          filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
          echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}";
          artifactPath = filesByGlob[0].path;
          artifactExists = fileExists artifactPath;
          if(artifactExists) {
            echo "***File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version: ${pom.version}";
            nexusArtifactUploader(
              nexusVersion: NEXUS_VERSION,
              protocol: NEXUS_PROTOCOL,
              nexusUrl: NEXUS_URL,
              groupId: pom.groupId,
              version: pom.version,
              repository: NEXUS_REPOSITORY,
              credentialsId: NEXUS_CREDENTIAL_ID,
              artifacts: [
                [artifactId: pom.artifactId,
                classifier: '',
                file: artifactPath,
                type: pom.packaging],
                [artifactId: pom.artifactId,
                classifier: '',
                file: "pom.xml",
                type: pom]
              ]
            );
          } else {
              error "***File: ${artifactPath}, could not be found";
          }
        }
      }
    }
  }
}
