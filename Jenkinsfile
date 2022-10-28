pipeline {
    agent { label 'tuto' }
    

    stages {
        stage('Build') {
            steps {
                container('podman') {
                    script {
                        sh 'podman build -t docker.io/nelsonyaccuzzi/web-go'
                    }
                }
            }
        }
    }
}