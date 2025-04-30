pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "your-dockerhub-username/hello-cicd"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/hello-cicd.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Test') {
            steps {
                echo 'No tests implemented yet...'
                // dir('backend') {
                //     sh 'npm test'
                // }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy stage (customize as needed)'
                // Example: SCP to a remote server, restart container
            }
        }
    }
}
