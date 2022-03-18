node('jenkins-maven') {
     stage('Running Maven') {
        sh(script: """
           echo "hello world"
           mvn --version
        """)
     }
}

node('jenkins-slave') {

     stage('unit-tests') {
        sh(script: """
            docker run --rm alpine /bin/sh -c "echo hello world"
        """)
    }
}
