node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("vchaudh3/webpage")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * Just an example */

        app.inside {
            sh 'echo "Tests passed"'
            sh 'cat /home/jenkins/remoting/logs/remoting.log.0'
            sh 'ps -ef'
            /*sh 'java -version'
            sh 'git config --list' */
            sh 'ls -lrtaR /home/jenkins/'
            sh 'ps -ef | grep -i nginx'
            sh 'netstat -tulpn'
            sh 'cat /home/jenkins/workspace/webpage/.git/config'
            /*sh 'cat /home/jenkins/workspace/webpage@tmp/durable*/*'*/
            sh 'sleep 100'

        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
