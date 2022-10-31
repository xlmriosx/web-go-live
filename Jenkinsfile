pipeline {
    agent { label 'tuto' }
    
    environment {
        DOCKERHUB_CREDS = credentials('dockerhub-credentials')
    }

    stages {
        stage('Build') {
            when {
                branch 'develop'
            }
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
            when {
                branch 'main'
            }
            steps {
                container('kubectl') {
                    script {
                        sh 'kubectl apply -f manifest.yaml'
                        sh 'kubectl rollout restart deployment -n web-go web-go'
                        sh 'kubectl rollout status deployment -n web-go web-go'
                    }
                }
            }
        }
    }
}
