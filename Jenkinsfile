node('jenkins-maven') {
     stage('Running Maven') {
        sh(script: """
           echo "hello world"
           mvn --version
           docker run -it --rm --name my-test -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven maven:3.3-jdk-8 mvn clean install
        """)
     }
}
