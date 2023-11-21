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
                        docker login -u $DOCKER_CREDS_USR -p $DOCKER_CREDS_PSW
                        docker build . -t bismabaig/nodejs-app:$BRANCH_NAME-latest -t bismabaig/nodejs-app:$BRANCH_NAME-$BUILD_ID
                        docker push bismabaig/nodejs-app:$BRANCH_NAME-latest
                        docker push bismabaig/nodejs-app:$BRANCH_NAME-$BUILD_ID
                    '''
                }
            }
        }
    }
    post { 
        success {
            sh "docker compose up -d "
        }
        failure { 
            echo 'Build failed!'
        }
    }
}
