pipeline {
    agent any
    tools {
        jdk 'java11' // Must match the JDK name in Jenkins
        maven 'myMaven'
        dockerTool 'myDocker'
    }
    environment {
        dockerHome = tool 'myDocker'
        mavenHome = tool 'myMaven'
        JAVA_HOME = tool 'java11'
        PATH = "$JAVA_HOME/bin:$dockerHome/bin:$mavenHome/bin:$PATH"
    }
    stages {
        stage('Build') {
            steps {
                sh '''
                    java --version
                    whoami
                    if [ -d /usr/lib/jvm ]; then ls -l /usr/lib/jvm; else echo "JVM directory not found"; fi
                    mvn --version
                '''
                sh 'docker --version'
                sh 'echo $PATH'
                sh 'mvn clean install -DskipTests' // Skip tests temporarily
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
