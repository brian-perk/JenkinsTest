#!groovy
pipeline {
    agent any

    stages { 
        stage('Deploy') {
            steps { 
                echo 'Deploying....'
                sshagent (credentials: ['jenkins']) {
                    def now = sh(returnStdout: true, script: 'date +%m%d%Y%H%M').trim()
                    echo now
                    sh 'now=$(date +%m%d%Y%H%M); ssh -o StrictHostKeyChecking=no jenkins@192.168.0.28 mkdir /var/www/vhosts/test/releases/$now; scp -ro StrictHostKeyChecking=no * jenkins@192.168.0.28:/var/www/vhosts/test/releases/$now; ssh -o StrictHostKeyChecking=no jenkins@192.168.0.28 ln -nsf /var/www/vhosts/test/releases/$now /var/www/vhosts/test/current'
                }
            }
        }
    }
}