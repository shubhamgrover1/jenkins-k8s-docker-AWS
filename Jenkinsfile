pipeline {
    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/shubhamgrover1/jenkins-k8s-docker-AWS.git'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    sh 'docker build -t shubhamgrover1/react-app:latest .'
                }
            }
        }

        stage('Login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKERHUB_PSW', usernameVariable: 'DOCKERHUB_USR')]) {
                        sh 'echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin'
                    }
                }
            }
        }

        stage('Push') {
            steps {
                sh 'docker push shubhamgrover1/react-app:latest'
            }
        }

        stage("Deploy to k3s") {
            steps {
                script {
                    sh """
                      kubectl apply -f deployment.yaml
                      kubectl apply -f service.yaml
                    """
                }
            }
        }
    }
}
