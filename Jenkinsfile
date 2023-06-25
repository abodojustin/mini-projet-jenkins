pipeline {
    environment {
        IMAGE_NAME = "simple-website"
        IMAGE_TAG = "v3"
        PREFIX_IMAGE = "abodojustin"
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
    }
}