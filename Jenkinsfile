pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS_ID = 'creds-dockerhub'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/YuYong22/Project_devops.git'
            }
        }
        stage('Build and Push Web Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKER_HUB_CREDENTIALS_ID) {
                        def webImage = docker.build("yuyong22/web:latest", "./web")
                        webImage.push()
                    }
                }
            }
        }
        stage('Build and Push App Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKER_HUB_CREDENTIALS_ID) {
                        def appImage = docker.build("yuyong22/app:latest", "./app")
                        appImage.push()
                    }
                }
            }
        }
        stage('Build and Push DB Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKER_HUB_CREDENTIALS_ID) {
                        def dbImage = docker.build("yuyong22/db:latest", "./db")
                        dbImage.push()
                    }
                }
            }
        }
    }
    post {
        success {
            emailext (
               // subject: 'Jenkins Build Successful: $PROJECT_NAME #$BUILD_NUMBER',
               // body: 'Tin tốt đây! Build cho job ${env.PROJECT_NAME} số build ${env.BUILD_NUMBER} đã thành công. Kiểm tra chi tiết tại đây ${env.BUILD_URL}.',
                body: 'bodyy #$BUILD_NUMBER $BUILD_URL', 
                subject: 'subjectt-success $PROJECT_NAME #$BUILD_NUMBER', 
                to: 'ndphongpc@gmail.com'
            )
        }
        failure {
            emailext (
              //  subject: 'Jenkins Build Failed: $PROJECT_NAME #$BUILD_NUMBER',
              //  body: 'Rất tiếc, build cho job ${env.PROJECT_NAME} số build ${env.BUILD_NUMBER} đã thất bại. Kiểm tra chi tiết tại đây ${env.BUILD_URL}.',
                body: 'bodyy #$BUILD_NUMBER $BUILD_URL', 
                subject: 'subjectt-failed $PROJECT_NAME #$BUILD_NUMBER', 
                to: 'ndphongpc@gmail.com'
            )
        }
    }
}
