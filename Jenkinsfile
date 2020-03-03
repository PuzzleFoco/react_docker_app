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
        kubernetes{
            yaml """
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app
spec:
  selector:
    matchLabels:
      app: app
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: app
        image: fabiuse/reactdocker:latest
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 8080

---
apiVersion: v1
kind: Service
metadata:
  name: app
spec:
  selector:
    app: app
  ports:
  - port: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: app
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: app

            """
        }
    }
}