pipeline {
    agent { label 'tuto' }
    

    stages {
        stage('Build') {
            steps {
                container('flyway') {
                    script {
                        sh 'flyway info -url="jdbc:mysql://mysql.mysql.svc.cluster.local:3306/devops" -user=elon -password=musk'
                    }
                }
            }
        }
    }
}
