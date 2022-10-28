pipeline {
    agent { label 'tuto' }
    

    stages {
        stage('Build') {
            steps {
                container('podman') {
                    script {
                        sh 'podman build -t docker.io/maxigoge/web-go -f Dockerfile'
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
