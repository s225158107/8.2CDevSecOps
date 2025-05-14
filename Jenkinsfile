pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/s225158107/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '"C:\\Program Files\\nodejs\\npm.cmd" install'
            }
        }

        stage('Run Tests') {
            steps {
                bat '"C:\\Program Files\\nodejs\\npm.cmd" test || exit /b 0'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat '"C:\\Program Files\\nodejs\\npm.cmd" run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat '"C:\\Program Files\\nodejs\\npm.cmd" audit || exit /b 0'
            }
        }
    }
}
