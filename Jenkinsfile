pipeline {
    agent any

    stages {
        stage('Initial Cleanup') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/mwangiii/php-todo.git'
            }
        }

        stage('Prepare Dependencies') {
            steps {
                sh 'mkdir -p bootstrap/cache'
                sh 'composer install'
                sh 'php artisan migrate --force'
                sh 'php artisan db:seed --force'
                sh 'php artisan key:generate'
                sh 'php artisan config:cache'
            }
        }

        stage('Execute Unit Tests') {
            steps {
                sh './vendor/bin/phpunit --configuration phpunit.xml'
            }
        }

        stage('Code Analysis') {
            steps {
                sh 'phploc app/ --log-csv build/logs/phploc.csv'
            }
        }
    }

}
