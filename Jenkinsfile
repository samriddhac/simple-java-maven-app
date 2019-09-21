pipeline {
	agent {
		docker {
			image 'maven:3-alpine'
			args '-v /home/ubuntu/.m2:/root/.m2'
		}
	}
	options {
		skipStagesAfterUnstable()
	}
	stages {
		stage('maven-test') {
			agent { docker 'openjdk:8-jre' } 
            steps {
                echo 'Hello, JDK'
                sh 'java -version'
            }
			post {
				always {
					echo 'always executed'
				}
				changed {
					echo 'changed executed'
				}
				fixed {
					echo 'fixed executed'
				}
				regression {
					echo 'regression executed'
				}
				aborted {
					echo 'aborted executed'
				}
				failure {
					echo 'failure executed'
				}
				success {
					echo 'success executed'
				}
				unstable {
					echo 'unstable executed'
				}
				unsuccessful {
					echo 'unsuccessful executed'
				}
				cleanup {
					echo 'cleanup executed'
				}
			}
		}
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
		stage('deploy') {
			steps {
				sh './jenkins/scripts/deliver.sh'
			}
		}
	}
}