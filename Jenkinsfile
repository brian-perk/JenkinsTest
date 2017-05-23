#!groovy
pipeline {
    agent any

    environment {
        def now = sh(returnStdout: true, script: 'date +%m%d%Y%H%M').trim()
    }

    stages { 
        stage('Deploy') {
            steps { 
                parallel (
                    "agent1" : {
                        steps {
                            sshagent (credentials: ['jenkins']) {
                            echo now
                            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.28 mkdir /var/www/vhosts/test/releases/$now' 
                            sh 'scp -ro StrictHostKeyChecking=no * jenkins@192.168.0.28:/var/www/vhosts/test/releases/$now' 
                            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.28 ln -nsf /var/www/vhosts/test/releases/$now /var/www/vhosts/test/current'
                                }
                            }
                        },
                    "agent2" : {
                        steps {
                            sshagent (credentials: ['jenkins']) {
                            echo now
                            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.32 mkdir /var/www/vhosts/test/releases/$now' 
                            sh 'scp -ro StrictHostKeyChecking=no * jenkins@192.168.0.32:/var/www/vhosts/test/releases/$now' 
                            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.32 ln -nsf /var/www/vhosts/test/releases/$now /var/www/vhosts/test/current'
                            }
                        }
                    }
                )
            }
        }
    }
}
