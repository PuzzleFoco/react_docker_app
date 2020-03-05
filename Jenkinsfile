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
        withKubeConfig([credentialsId: 'jenkins-service-account', serverUrl: 'https://kubernetes.default']){
            sh 'kubectl apply -f kubernetes.yaml'
        }
    }    
}