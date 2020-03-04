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
        withCredentials([kubeconfigContent(credentialsId: 'f47efbc2-978b-4558-9f40-7abbb0fec994', variable: 'KUBECONFIG_CONTENT')]) {
            sh '''echo "$KUBECONFIG_CONTENT" > kubeconfig && cat kubeconfig && rm kubeconfig'''
        }
    }
}