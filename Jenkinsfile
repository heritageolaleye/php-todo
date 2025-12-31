pipeline {
    agent any

    stages {

        stage('Initial cleanup') {
            steps {
                dir("${WORKSPACE}") {
                    deleteDir()
                }
            }
        }

        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/StegTechHub/php-todo.git'
            }
        }

        stage('Prepare Dependencies') {
            steps {
                // Rename .env.sample to .env
                sh 'cp .env.sample .env'

                // Install PHP dependencies
                sh 'composer install --no-interaction --prefer-dist'

                // Run Laravel migrations
                sh 'php artisan migrate --force'

                // Seed the database
                sh 'php artisan db:seed --force'

                // Generate application key
                sh 'php artisan key:generate --force'
            }
        }

        stage('Execute Unit Tests') {
            steps {
                // Run PHPUnit tests
                sh './vendor/bin/phpunit'
            }
        }

        stage('Code Analysis') {
            steps {
                // Create build/logs directory if it doesn't exist
                sh 'mkdir -p build/logs'

                // Run PHPLoc and save output to CSV
                sh 'phploc app/ --log-csv build/logs/phploc.csv'
            }
        }
    }

  }
}
