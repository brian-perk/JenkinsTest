pipeline {
    agent {
        docker { image 'node:7-alpine' }
    }
    stages {
        stage('Test') {
            steps {
                sh 'node --version'
		node {
			echo "Hello World"
		}
            }
        }
    }
}
