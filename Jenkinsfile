pipeline {
    agent any
    environment {
        dockerHome = tool 'myDocker'
        mavenHome = tool 'myMaven'
        PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/abhijeet9sh/jenkins-devops-microservices-pipeline.git', branch: 'main'
                echo 'Checked out code from Git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn --version'
                sh 'docker --version'
                sh 'mvn clean install'
                echo "Build completed - BUILD_NUMBER: $env.BUILD_NUMBER"
                echo "Job: $env.JOB_NAME, Tag: $env.BUILD_TAG"
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
                echo 'Tests completed'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-microservice:$env.BUILD_NUMBER .'
                echo 'Docker image built'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                    sh 'docker push my-microservice:$env.BUILD_NUMBER'
                }
                echo 'Docker image pushed'
            }
        }
    }
    post {
        always {
            echo 'Pipeline execution completed'
        }
        success {
            echo 'Pipeline succeeded'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
