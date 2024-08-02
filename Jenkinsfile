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
                body: """
                    <p>Tin tốt đây!</p>
                    <p>Build cho job <b>${env.JOB_NAME}</b> số build <b>${env.BUILD_NUMBER}</b> đã thành công.</p>
                    <p>Kiểm tra chi tiết <a href="${env.BUILD_URL}">tại đây</a>.</p>
                """,
                to: env.RECIPIENTS,
                mimeType: 'text/html'
            )
        }
        failure {
            emailext (
                subject: "Jenkins Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    <p>Rất tiếc, build cho job <b>${env.JOB_NAME}</b> số build <b>${env.BUILD_NUMBER}</b> đã thất bại.</p>
                    <p>Kiểm tra chi tiết <a href="${env.BUILD_URL}">tại đây</a>.</p>
                """,
                to: env.RECIPIENTS,
                mimeType: 'text/html'
            )
        }
    }
// cách 2
    //  post {
    //     success {
    //         emailext (
    //             body: 'bodyy #$BUILD_NUMBER $BUILD_URL', 
    //             subject: 'subjectt-success $JOB_NAME #$BUILD_NUMBER', 
    //             to: 'phong3baox@gmail.com'
    //         )
    //     }
    //     failure {
    //         emailext (
    //             body: 'bodyy #$BUILD_NUMBER ${env.BUILD_URL}', 
    //             subject: 'subjectt-failed ${env.JOB_NAME} #${env.BUILD_NUMBER}', 
    //             to: 'phong3baox@gmail.com'
    //         )
    //     }
    // }
}
