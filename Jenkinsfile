pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "bytemaster2704/nodeapp"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Test') {
            steps {
                bat 'npm test'
            }
        }

        stage('Package') {
            steps {
                bat 'mkdir dist'
                bat 'echo Packaging successful > dist\\info.txt'
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }

        stage('Docker Build') {
            steps {
                bat "docker build -t %DOCKER_IMAGE%:%BUILD_NUMBER% ."
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat "docker login -u %USER% -p %PASS%"
                }
                bat "docker tag %DOCKER_IMAGE%:%BUILD_NUMBER% %DOCKER_IMAGE%:latest"
                bat "docker push %DOCKER_IMAGE%:%BUILD_NUMBER%"
                bat "docker push %DOCKER_IMAGE%:latest"
            }
        }

        stage('Deploy (Simulated)') {
            steps {
                bat 'echo Deploy complete'
            }
        }
    }
}
