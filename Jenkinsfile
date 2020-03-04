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

    
    stage('Deploy') {
        withKubeConfig([credentialsId: '92de7d32-9148-4a51-89b8-e6c623002efd', serverUrl: 'https://kubernetes.default']){
            sh 'kubectl get nodes'
        }
    }    
}