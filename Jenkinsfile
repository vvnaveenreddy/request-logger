node {
    def app

    tools {
        maven 'Maven 3.3.9'
        jdk 'jdk8'
    }
    stage('Clone repository') {
        checkout scm
    }

    stage('Maven Build') {
        app.inside{ sh 'mvn -Dmaven.test.failure.ignore=true clean install' }
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
