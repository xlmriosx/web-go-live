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
                        sh 'podman login -u $DOCKERHUB_CREDS_USR -p DOCKERHUB_CREDS_PSW'
                        sh 'podman push docker.io/nelsonyaccuzzi/web-go:$BUILD_NUMBER'
                    }
                }
            }
        }
    }
}
