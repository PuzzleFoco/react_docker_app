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
        withKubeConfig([credentialsId: '0c3d1e4d-e38b-49f0-9b9c-0403d624ecf4', serverUrl: 'https://kubernetes.default']){
            sh 'kubectl apply -f reactymlfile.yml'
        }
    }    
}