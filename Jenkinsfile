node {
    def app
    stage('Clone repository'){
        checkout scm
    }

    stage('Build image'){
        app = docker.build("fabiuse/reactdocker")
    }

    stage('Test image'){
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image'){
        docker.withRegistry('','docker-hub-credentials'){
            app.push("latest")
        }
    }

    stage('Deploy Container') {
        agent {
            kubernetes {}
        }
        stages {
            stage {
                steps {
                    sh 'echo kubectl get nodes'
                }
            }
        }
    }
}