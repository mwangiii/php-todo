pipeline {
    agent any

    stages {

        stage("Initial Cleanup") {
            steps {
                dir("${WORKSPACE}") {
                    deleteDir()
                }
            }
        }

        stage("Checkout SCM") {
            steps {
                git branch: 'main', url: 'https://github.com/mwangiii/php-todo.git'
            }
        }

        stage("Prepare Dependencies") {
            steps {
                sh 'mv .env.sample .env'

                // Ensure cache directories exist and have proper permissions
                sh 'mkdir -p bootstrap/cache'
                sh 'chmod -R 775 bootstrap/cache'

                sh 'mkdir -p storage/logs'
                sh 'chmod -R 775 storage'

                // Install dependencies
                sh 'composer install --no-interaction --prefer-dist'

                // Clear and cache configuration to prevent runtime errors
                sh 'php artisan config:clear'
                sh 'php artisan config:cache'

                // Optional: Clear other caches
                sh 'php artisan route:clear'
                sh 'php artisan view:clear'

                // Run migrations and seeders
                sh 'php artisan migrate --force'
                sh 'php artisan db:seed --force'

                // Generate application key
                sh 'php artisan key:generate'
            }
        }
    }
}
