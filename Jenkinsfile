pipeline {
	agent {
		docker {
			image 'maven:3-alpine'
			args '-v /home/ubuntu/.m2:/root/.m2'
		}
	}
	stages {
		stage('build') {
			steps {
				sh 'mvn clean package -Dmaven.skipTests=true'
			}
		}
		stage('test') {
			steps {
				sh 'mvn test'
			}
			post {
				always {
					junit 'target/surefire-reports/*.xml'
				}
			}
		}
	}
}