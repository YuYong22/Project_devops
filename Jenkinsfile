pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS_ID = 'creds-dockerhub'
        RECIPIENTS = 'phong3baox@gmail.com'
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
                subject: "Jenkins Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """Tin tốt đây!
                        Build cho job ${env.JOB_NAME} số build ${env.BUILD_NUMBER} đã thành công.
                        Kiểm tra chi tiết tại đây ${env.BUILD_URL}""",
                to: "${RECIPIENTS}"
            )
        }
        failure {
            emailext (
                subject: "Jenkins Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """Rất tiếc, build cho job ${env.JOB_NAME} số build ${env.BUILD_NUMBER} đã thất bại.
                        Kiểm tra chi tiết tại đây ${env.BUILD_URL}.""",
                to: "${RECIPIENTS}"
            )
        }
    }
}
