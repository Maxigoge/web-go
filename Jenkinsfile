pipeline {
    agent { label 'tuto' }
    
    environment {
        DOCKERHUB_CREDS = credentials('webo-credentials')
    }

    stages {
        
        stage('Build') {
          when {
            allOf {
                branch 'PR-*'
                environment name: 'CHANGE_TARGET', value: 'main'
                }
            }
            steps {
                container('podman') {
                    script {
                        sh 'podman build -t docker.io/maxigoge/web-go:$BUILD_NUMBER -f Dockerfile'
                    }
                }
            }
        }
        
        
        stage('Build') {
            when {
                branch 'main'
            }
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
