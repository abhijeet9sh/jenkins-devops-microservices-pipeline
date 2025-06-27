//SCRIPTED

//DECLARATIVE
pipeline {
	agent any
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$dockerHome/bin:$PATH"
	}
	stages {
		stage ('Build') {
		steps {
			sh 'mvn --version'
			sh 'docker --version'
			echo "Build"
			echo "PATH - $PATH"
			echo "BUILD_NUMBER - $env.BUILD_NUMBER"
			echo "JOB_NAME - $env.JOB_NAME"
			echo "BUILD_TAG - $env.BUILD_TAG"


		}
		}
		stage('Test') {
			steps {
				echo "TEST COMPLETED"
			}
		}
	}
}