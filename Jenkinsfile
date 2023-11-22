pipeline {
    agent {
        label 'labk8z'
    }
    environment {
        DOCKER_CREDS = credentials('dockerhub_id')
        PORT_DEV = '4040'
        MONGODB_URI_DEV = credentials('product_dev')
        PORT_PROD = '5050'
        MONGODB_URI_PROD = credentials('product_prod')
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
            script {
                if (BRANCH_NAME == 'dev') {
                    sh '''
                        echo "PORT=${PORT_DEV}" >> .env 
                        echo "MONGODB_URI=${MONGODB_URI_DEV}" >> .env
                    '''
                } else {
                    sh '''
                        echo "PORT=${PORT_PROD}" >> .env 
                        echo "MONGODB_URI=${MONGODB_URI_PROD}" >> .env
                    '''
                }
                sh 'docker compose up -d'
            }
        }
        failure { 
            echo 'Build failed!'
        }
    }
}
