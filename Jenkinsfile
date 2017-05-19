#!groovy
pipeline {
    agent any

    stages { 
        stage('Deploy') {
            steps { 
                echo 'Deploying....'
                sshagent (credentials: ['jenkins']) {
                    sh 'now=$(date +%m%d%Y%H%M)'
                    sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.28 mkdir /var/www/vhosts/test/releases/$(now)'
                    sh 'scp -ro StrictHostKeyChecking=no * jenkins@192.168.0.28:/var/www/vhosts/test/releases/$(now)'
                }
            }
        }
    }
}