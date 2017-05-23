#!groovy
pipeline {
    agent any

    environment {
        def now = sh(returnStdout: true, script: 'date +%m%d%Y%H%M').trim()
    }

    stages { 
        stage('Info') {
            steps {
                echo "Release: " now
            }
        }
        stage('Deploy') {
            steps { 
                parallel (
                    "agent1" : {
                            sshagent (credentials: ['jenkins']) {
                            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.28 mkdir /var/www/vhosts/test/releases/$now' 
                            sh 'scp -ro StrictHostKeyChecking=no * jenkins@192.168.0.28:/var/www/vhosts/test/releases/$now' 
                            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.28 ln -nsf /var/www/vhosts/test/releases/$now /var/www/vhosts/test/current'
                            }
                    },
                    "agent2" : {
                            sshagent (credentials: ['jenkins']) {
                            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.31 mkdir /var/www/vhosts/test/releases/$now' 
                            sh 'scp -ro StrictHostKeyChecking=no * jenkins@192.168.0.31:/var/www/vhosts/test/releases/$now' 
                            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.31 ln -nsf /var/www/vhosts/test/releases/$now /var/www/vhosts/test/current'
                            }
                    }
                )
            }
        }
    }
}
