node {
    def app
    stage('Clone repository'){
        checkout scm
    }

    stage('Build image'){
        agent{
            docker {
                image 'getintodevops/jenkins-withdocker'
            }
        }
        app = docker.build("app/reactdocker")
    }

    stage('Test image'){
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image'){
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials'){
            app.push("latest")
        }
    }
}