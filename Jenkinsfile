pipeline {
    agent any

  stages {

     stage("Initial cleanup") {
          steps {
            dir("${WORKSPACE}") {
              deleteDir()
            }
          }
        }

    stage('Checkout SCM') {
      steps {
            git branch: 'main', url: 'https://github.com/mwangiii/php-todo.git'
      }
    }

    stage('Prepare Dependencies') {
      steps {
             sh'php artisan config:clear'
             sh'php artisan cache:clear'
             sh'php artisan view:clear'
             sh'php artisan config:cache'
             sh'php artisan route:cache'
             sh'php artisan view:cache'
             sh 'mv .env.sample .env'
             sh 'mkdir -p bootstrap/cache'
             sh 'composer install'
             sh 'php artisan migrate'
             sh 'php artisan db:seed'
             sh 'php artisan key:generate'
      }
    }
  }
}