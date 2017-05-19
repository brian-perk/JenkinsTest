#!groovy
pipeline {
    agent any

    stages { 
        stage('Deploy') {
            steps { 
                echo 'Deploying....'
                sshagent (credentials: ['jenkins']) {
                    sh 'ssh -o StrictHostChecking=no -l jenkins 192.168.0.28 uname -a'
            }
        }
    }
}