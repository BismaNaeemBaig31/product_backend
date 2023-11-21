pipeline {
    agent {
        label 'new_server'
    }
    environment {
        DOCKER_CREDS = credentials('dockerhub_id')
        PORT = '4040'
        MONGODB_URI = "mongodb+srv://bismanaeembaig:6f5y6BoPibaoWFS5@cluster0.p5pxm7a.mongodb.net/?retryWrites=true&w=majority"
    }
    stages {
        stage('Build') {
            steps {
                script {
                    sh '''
                        echo "PORT: \${PORT}"
                        echo "MONGODB_URI: \${MONGODB_URI}"
                    '''
                }
            }
        }
    }
    post { 
        success {
            echo 'Build success'   
        }
        failure { 
            echo 'Build failed!'
        }
    }
}
