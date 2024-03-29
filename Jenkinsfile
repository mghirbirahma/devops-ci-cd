pipeline {
    agent none
    environment {
        // Add the dh_cred variable for authentication
        DOCKERHUB_CREDENTIALS = credentials('dh_cred') 
    }
       
    stages {
        stage('Checkout') {
            agent any
            steps {
                checkout scm
            }
        }

        stage('Init') {
            agent any
            steps {
                // Authenticate with Docker Hub
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Build Backend') {
            agent any
            //when {
               // changeset "**/backend/*.*"
            //}
            steps {
                dir('Backend') {
                    sh 'docker build -t $DOCKERHUB_CREDENTIALS_USR/produit_backend:$BUILD_ID .'
                    sh 'docker push $DOCKERHUB_CREDENTIALS_USR/produit_backend:$BUILD_ID'
                    sh 'docker rmi $DOCKERHUB_CREDENTIALS_USR/produit_backend:$BUILD_ID'
                } 
            }
        }

        stage('Build Frontend') {
            agent any
            //when {
              //  changeset "**/frontend/*.*"
           // }
            steps {
                dir('frontend') {
                    sh 'docker build -t $DOCKERHUB_CREDENTIALS_USR/produit_frontend:$BUILD_ID .'
                    sh 'docker push $DOCKERHUB_CREDENTIALS_USR/produit_frontend:$BUILD_ID'
                    sh 'docker rmi $DOCKERHUB_CREDENTIALS_USR/produit_frontend:$BUILD_ID'
                } 
            }
        }

        stage('Logout') {
            agent any
            steps {
                sh 'docker logout'
            }
        }
    }
}