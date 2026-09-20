pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Source code checked out by Jenkins SCM'
            }
        }

        stage('Frontend Lint') {
            steps {
                dir('mern/client') {
                    sh 'npm ci'
                    sh 'npm run lint'
                }
            }
        }

        stage('Frontend Build') {
            steps {
                dir('mern/client') {
                    sh 'npm run build'
                }
            }
        }

        stage('Backend Install') {
            steps {
                dir('mern/server') {
                    sh 'npm ci'
                }
            }
        }

        stage('Docker Build') {
    steps {
        sh 'docker build -t affaana/mern-backend:${BUILD_NUMBER} ./mern/server'
        sh 'docker build -t affaana/mern-frontend:${BUILD_NUMBER} ./mern/client'
    }
}

stage('Docker Push') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'DOCKER_USERNAME',
            passwordVariable: 'DOCKER_PASSWORD'
        )]) {
            sh '''
                echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin

                docker push "affaana/mern-backend:${BUILD_NUMBER}"
                docker push "affaana/mern-frontend:${BUILD_NUMBER}"

                docker logout
            '''
        }
    }
}
    }
}
