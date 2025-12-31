pipeline {
    agent any

    stages {

        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/StegTechHub/php-todo.git'
            }
        }

        stage('Initial cleanup') {
            steps {
                // Optional: delete everything except .env.sample
                sh 'find . ! -name ".env.sample" -delete'
            }
        }

        stage('Prepare Dependencies') {
            steps {
                sh 'cp .env.sample .env'
                sh 'composer install --no-interaction --prefer-dist'
                sh 'php artisan migrate --force'
                sh 'php artisan db:seed --force'
                sh 'php artisan key:generate --force'
            }
        }

        stage('Execute Unit Tests') {
            steps {
                sh './vendor/bin/phpunit'
            }
        }

        stage('Code Analysis') {
            steps {
                sh 'mkdir -p build/logs'
                sh 'phploc app/ --log-csv build/logs/phploc.csv'
            }
        }
    }
}

