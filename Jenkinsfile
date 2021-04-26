node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Maven Build') {
        app.inside{ sh 'echo "maven build "' }
    }

    stage('Build image') {
        app = docker.build("brainupgrade/request-logger")
    }
    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
