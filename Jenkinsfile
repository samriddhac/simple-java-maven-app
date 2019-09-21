pipeline {
	agent {
		docker {
			image 'maven:3-alpine'
			args '-v /home/ubuntu/.m2:/root/.m2'
		}
	}
	triggers {
        cron('H */4 * * 1-5')
    }
	options {
		timeout(time: 1, unit: 'HOURS')
		timestamps()
		skipStagesAfterUnstable()
		retry(3)
	}
	/*Stages in Declarative Pipeline may declare a list of nested stages 
	to be run within them in sequential order. Note that a stage must have one and only one of steps, 
	parallel, or stages, the last for sequential stages. It is not possible to nest a parallel block 
	within a stage directive if that stage directive is nested within a parallel block itself. 
	However, a stage directive within a parallel block can use all other functionality of a stage, 
	including agent, tools, when, etc*/
	stages {
		stage('maven-test') {
			/*agent { docker 'openjdk:8-jre' }*/
			environment {
				defaultEnv = 'development'
			}
            steps {
                echo 'Maven step'
                sh 'mvn --version'
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
			when {
                branch 'master'
                environment name: 'DEPLOY_TO', value: 'dev'
            }
			steps {
				sh './jenkins/scripts/deliver.sh'
			}
		}
	}
}