pipeline {
    agent { label 'tuto' }
    
    environment {
        DOCKERHUB_CREDS = credentials('webo-credentials')
    }

    stages {
        stage('Build') {
            steps {
                container('podman') {
                    script {
                        sh 'podman build -t docker.io/maxigoge/web-go:$BUILD_NUMBER -f Dockerfile'
                        sh 'podman login docker.io -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW'
                        sh 'podman tag docker.io/maxigoge/web-go:$BUILD_NUMBER docker.io/maxigoge/web-go:latest'
                        sh 'podman push docker.io/maxigoge/web-go:$BUILD_NUMBER'
                        sh 'podman push docker.io/maxigoge/web-go:latest'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                container('kubectl') {
                    script {
                        sh 'kubectl apply -f manifest.yaml'
                        sh 'kubectl rollout restart deployment -n maxigoge web-go'
                        sh 'kubectl rollout status deployment -n maxigoge web-go'
                    }
                }
            }
        }
    }
}
