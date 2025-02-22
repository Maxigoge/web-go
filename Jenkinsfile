pipeline {
    agent { label 'tuto' }
    
    environment {
        DOCKERHUB_CREDS = credentials('webo-credentials')
    }

    stages {
        
        stage('Build-develop') {
            when {
                branch 'develop'
            }
            steps {
                container('podman') {
                    script {
                        sh 'podman build -t docker.io/maxigoge/web-go:$BUILD_NUMBER -f Dockerfile'
                        sh 'podman tag docker.io/maxigoge/web-go:$BUILD_NUMBER docker.io/maxibadaro/web-go:dev'
                        sh 'podman login docker.io -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW'
                        sh 'podman push docker.io/maxigoge/web-go:$BUILD_NUMBER'
                        sh 'podman push docker.io/maxigoge/web-go:dev'
                    }
                }
            }
        }
        stage('Deploy-develop') {
            when {
                branch 'develop'
            }
            steps {
                container('kubectl') {
                    script {
                        sh 'kubectl apply -f dev.yaml'
                        sh 'kubectl rollout restart deployment -n dev web-go'
                        sh 'kubectl rollout status deployment -n dev web-go'
                    }
                }
            }
        }
                stage('Build-main') {
            when {
                branch 'main'
            }
            steps {
                container('podman') {
                    script {
                        sh 'podman login docker.io -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW'
                        sh 'podman pull docker.io/maxigoge/web-go:dev'
                        sh 'podman tag docker.io/maxigoge/web-go:dev docker.io/maxibadaro/web-go:latest'
                        sh 'podman push docker.io/maxigoge/web-go:latest'
                    }
                }
            }
        }
        stage('Deploy-main') {
            when {
                branch 'main'
            }
            steps {
                container('kubectl') {
                    script {
                        sh 'kubectl apply -f todo.yaml'
                        sh 'kubectl rollout restart deployment -n maxigoge web-go'
                        sh 'kubectl rollout status deployment -n maxigoge web-go'
                    }
                }
            }
        }
    }
}
