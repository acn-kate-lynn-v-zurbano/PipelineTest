pipeline {
	agent none
	stages {
		stage('Initialize') {
			agent any
			steps {
				sh "printenv"
			}
		}
		stage('Test') {
			agent any
			steps {
				checkout scm: [
					$class: 'GitSCM',
					userRemoteConfigs: [[credentialsId: 'github-cred', url: GIT_URL]],
					branches: [[name: GIT_COMMIT]]
				]
			}
		}
		stage('Status') {
            agent any
            steps {
                sh "echo 'DONE'"
            }
        }
	}
    options {
        buildDiscarder(logRotator(numToKeepStr:'10'))
        timeout(time: 60, unit: 'MINUTES')
        disableConcurrentBuilds()
    }
}
