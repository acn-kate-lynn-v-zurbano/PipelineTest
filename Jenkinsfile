pipeline {
	agent none
	stages {
		stage('Initialize') {
			agent any
			steps {
				sh "printenv"
                sh '''
                    yum install nettools -y
                    netstat -tuanop | grep 5000'''
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
		        logstashSend failBuild: true, maxLines: 1000
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
