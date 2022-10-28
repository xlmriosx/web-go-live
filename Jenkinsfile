pipeline {
    agent { label 'tuto' }
    
    environment {
        DOCKERHUB_CREDS = credentials('dockerhub-credentials')
    }

    stages {
        stage('Build') {
            steps {
                container('podman') {
                    script {
                        sh 'podman build -t docker.io/nelsonyaccuzzi/web-go:$BUILD_NUMBER -f Dockerfile'
                        sh 'podman login docker.io -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW'
                        sh 'podman tag docker.io/nelsonyaccuzzi/web-go:$BUILD_NUMBER docker.io/nelsonyaccuzzi/web-go:latest'
                        sh 'podman push docker.io/nelsonyaccuzzi/web-go:$BUILD_NUMBER'
                        sh 'podman push docker.io/nelsonyaccuzzi/web-go:latest'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                container('kubectl') {
                    script {
                        sh 'kubectl apply -f manifest.yaml'
                    }
                }
            }
        }
    }
}
