pipeline {
    agent { label 'node2' }  // Run the pipeline on agent labeled 'node2'

    environment {
        IMAGE_NAME = 'sreyas3599/my-image'
        TAG = 'latest'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Will use default SCM checkout since the Jenkinsfile is in the repo
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${TAG}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh """
                            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                            docker push ${IMAGE_NAME}:${TAG}
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}
