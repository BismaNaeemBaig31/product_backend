pipeline {
    agent {
        label 'labk8z'
    }
    environment {
        DOCKER_CREDS = credentials('dockerhub_id')
        PORT = '4040'
        MONGODB_URI = credentials('product_dev')
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
            sh ''' 
             echo  "PORT=${PORT}" >> .env 
             echo "MONGODB_URI=${MONGODB_URI}" >> .env
            docker compose up -d
            '''
        }
        failure { 
            echo 'Build failed!'
        }
    }
}
