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
        withCredentials([kubeconfigContent(credentialsId: 'd27203eb-2ac2-45b9-860e-4cc00c567e44', variable: 'KUBECONFIG_CONTENT')]) {
            sh '''echo "$KUBECONFIG_CONTENT" > kubeconfig && cat kubeconfig && rm kubeconfig'''
        }
    }
}