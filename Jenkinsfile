pipeline {
	agent { node { label 'lisk-docker' } }
	stages {
		stage('checkout') {
			steps {
				checkout scm
			}
		}
		stage('docker build') {
			steps {
				sh 'make images'
			}
		}
		stage('smoke tests') {
			steps {
				ansiColor('xterm') {
					sh 'make testnet'
					dir('testnet') {
						sh 'docker-compose logs redis |grep --quiet "Ready to accept connections"'
						sh 'docker-compose logs db |grep --quiet "PostgreSQL init process complete; ready for start up."'
						sh 'docker-compose logs lisk |grep --quiet "Database is ready."'
					}
				}
			}
		}
	}
	post {
	    always {
		ansiColor('xterm') {
			sh 'make -C testnet mrproper'
		}
	    }
	}
}
