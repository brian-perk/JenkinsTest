#!groovy
pipeline {
    agent any

    environment {
        def now = sh(returnStdout: true, script: 'date +%m%d%Y%H%M').trim()
    }

    stages { 
        stage('Info') {
            steps {
                sh 'echo Release Number: $now'
            }

        stage('Add config file') {
            steps {
                configFileProvider([configFile(fileId: '10b17903-85a0-43e9-a877-c18defe61a55', targetLocation: '.env', variable: 'combre.env')])
            }
        }

        }
        stage('Deploy') {
            steps { 
                parallel (
                    "agent1" : {
                            sshagent (credentials: ['jenkins']) {
                                sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.28 mkdir -p /var/www/vhosts/test/releases/$now'
                                sh 'scp -ro StrictHostKeyChecking=no . jenkins@192.168.0.28:/var/www/vhosts/test/releases/$now'
                                sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.28 ln -nsf /var/www/vhosts/test/storage /var/www/vhosts/test/releases/$now/storage'
                                //sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.28 chmod -R a+rwx /var/www/vhosts/test/releases/$now/bootstrap/cache'
                                //sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.28 "cd /var/www/vhosts/test/releases/$now; php -d apc.enable_cli=0 composer.phar install --prefer-dist --no-interaction"'
                                //sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.28 "cd /var/www/vhosts/test/releases/$now; php artisan migrate --force"'
                                sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.28 ln -nsf /var/www/vhosts/test/releases/$now /var/www/vhosts/test/current'

                            }
                    },
                    "agent2" : {
                            sshagent (credentials: ['jenkins']) {
                                sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.31 mkdir /var/www/vhosts/test/releases/$now' 
                                sh 'scp -ro StrictHostKeyChecking=no . jenkins@192.168.0.31:/var/www/vhosts/test/releases/$now'
                                sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.31 ln -nsf /var/www/vhosts/test/storage /var/www/vhosts/test/releases/$now/storage'
                                //sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.31 chmod -R a+rwx /var/www/vhosts/test/releases/$now/bootstrap/cache'
                                //sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.31 "cd /var/www/vhosts/test/releases/$now; php -d apc.enable_cli=0 composer.phar install --prefer-dist --no-interaction"'
                                //sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.31 "cd /var/www/vhosts/test/releases/$now; php artisan migrate --force"'
                                sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.0.31 ln -nsf /var/www/vhosts/test/releases/$now /var/www/vhosts/test/current'
                            }
                    }
                )
            }
        }
    }
}
