pipeline {
    agent any

    tools {
        maven 'maven'       // Name of your Maven tool in Jenkins
    }

    environment {
        TAG = "1.0"
        IMAGE = "sharonkumar/insure-me"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/sharonkumar1/insure-me-project.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Publish Test Reports') {
            steps {
                publishHTML(target: [
                    reportDir: 'target/surefire-reports',
                    reportFiles: 'index.html',
                    reportName: 'Test Report'
                ])
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE:$TAG .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: 'Dockerhub-creds', url: 'https://index.docker.io/v1/']) {
                    sh 'docker push $IMAGE:$TAG'
                }
            }
        }

        stage('Deploy to Test Server') {
            steps {
                echo 'Waiting for Ansible deployment step – will configure once inventory is ready.'
            }
        }
    }
}
