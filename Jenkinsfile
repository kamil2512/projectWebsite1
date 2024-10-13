pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'kamil266/projectwebsite'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/kamil2512/projectWebsite1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'e95975a5-5a04-44c2-a69f-adcae50f365f') {
                        docker.image("${DOCKER_IMAGE}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                sh 'docker run -d -p 8080:80 ${DOCKER_IMAGE}:${env.BUILD_NUMBER}'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}