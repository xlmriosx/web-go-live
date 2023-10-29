pipeline {
    agent { label 'tuto' }
    
    environment {
        DOCKERHUB_CREDS = credentials('dockerhub-credentials')
        REPOSITORY = "nelsonyaccuzzi/web-go"
    }

    stages {
               
         stage('Build on PR') {
          when {
            allOf {
                branch 'PR-*'
                environment name: 'CHANGE_TARGET', value: 'main'
                }
            }
            steps {
                container('podman') {
                    script {
                        sh 'podman build -t docker.io/$REPOSITORY:$BUILD_NUMBER -f Dockerfile'
                    }
                }
            }
        }
        stage('Build on main') {
            when {
                branch 'main'
            }
            steps {
                container('podman') {
                    script {
                        sh 'podman build -t docker.io/$REPOSITORY:$BUILD_NUMBER -f Dockerfile'
                        sh 'podman login docker.io -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW'
                        sh 'podman tag docker.io/$REPOSITORY:$BUILD_NUMBER docker.io/$REPOSITORY:latest'
                        sh 'podman push docker.io/$REPOSITORY:$BUILD_NUMBER'
                        sh 'podman push docker.io/$REPOSITORY:latest'
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
