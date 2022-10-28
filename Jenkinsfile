pipeline {
    agent { label 'tuto' }
    
    environment {
        DOCKERHUB_CREDS = credentials('webgo-credentials')
    }

    stages {
        stage('Build') {
            steps {
                container('podman') {
                    script {
                        sh 'podman build -t docker.io/maxigoge/web-go:$BUILD_NUMBER -f Dockerfile'
                        sh 'podman login -u $DOCKERHUB_CREDS_USR -p DOCKERHUB_CREDS_PSW'
                        sh 'podman push docker.io/maxigoge/web-go:$BUILD_NUMBER'
                    }
                }
                container('kubectl') {
                    script {
                        sh 'env'
                        sh 'kubectl get pod'
                    }
                }
                container('fortune') {
                    script {
                        sh 'fortune'
                    }
                }
            }
        }
    }
}
