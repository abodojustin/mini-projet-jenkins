pipeline {
    environment {
        IMAGE_NAME = "simple-website"
        IMAGE_TAG = "v3"
        PREFIX_IMAGE = "abodojustin"
        DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB_KEY')
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }
    agent none
    stages {
        stage('Build Image') {
            agent any
            steps {
                script {
                    sh '''
                        docker build -t $PREFIX_IMAGE/$IMAGE_NAME:$IMAGE_TAG .
                    '''
                }
            }
        }
        stage('Code Quality') {
            agent any
            steps {
                script {
                    sh '''
                        docker run --name $IMAGE_NAME -d -p 80:80 $PREFIX_IMAGE/$IMAGE_NAME:$IMAGE_TAG
                        sleep 10
                    '''
                }
            }
        }
        stage('Test') {
            agent any
            steps {
                script {
                    sh '''
                        curl http://3.95.39.174 | grep -q "Dimension"
                    '''
                }
            }
        }
        stage('Package') {
            agent any
            steps {
                script {
                    sh '''
                        docker rm -f ${IMAGE_NAME}
                        echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                        docker push $PREFIX_IMAGE/$IMAGE_NAME:$IMAGE_TAG
                    '''
                }
            }
        }
        stage('Staging') {
            when {
                expression { GIT_BRANCH == 'origin/master' }
            }
            agent any
            steps {
                script {
                    sh '''
                        aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                        aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                        aws configure set region us-east-1
                        eb init --region us-east-1 --platform Docker website-staging_$BUILD_NUMBER
                        eb create staging-env-$BUILD_NUMBER || echo "already exists."
                        eb status
                    '''
                }
            }
        }
    }
}