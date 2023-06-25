pipeline {
    environment {
        IMAGE_NAME = "simple-website"
        IMAGE_TAG = "v3"
        PREFIX_IMAGE = "abodojustin"
        DOCKERHUB_CREDENTIALS=credentials('DOCKERHUB_KEY')
    }
    agent none
    stages {
        stage('Build Image') {
            agent any
            steps {
                script {
                    sh 'docker build -t $PREFIX_IMAGE/$IMAGE_NAME:$IMAGE_TAG .'
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
                        curl http://3.95.39.174"
                    '''
                }
            }
        }
        stage('Package') {
            agent any
            steps {
                script {
                    sh '''
                        docker tag $IMAGE_NAME $PREFIX_IMAGE/$IMAGE_NAME:$IMAGE_TAG
                        echo $DOCKERHUB_CREDENTIALS | docker login -u $DOCKERHUB_CREDENTIALS --password-stdin'
                        docker push --all-tags ${IMAGE_NAME}
                    '''
                }
            }
        }
    }
}