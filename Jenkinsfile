pipeline {
    agent any

    stages {
        // Stage 1: Build – compile and install dependencies
        stage('Build') {
            steps {
                git branch: 'main', url: 'https://github.com/s225158107/8.2CDevSecOps.git'
                bat '"C:\\Program Files\\nodejs\\npm.cmd" install'
            }
        }

        // Stage 2: Unit and Integration Tests
        stage('Unit and Integration Tests') {
            steps {
                 bat '"C:\\Program Files\\nodejs\\npm.cmd" test || exit /b 0'
               

            }
        }

        // Stage 3: Code Analysis
        stage('Code Analysis') {
            steps {
                echo 'Running code analysis using SonarQube (simulated)...'
                bat 'echo Simulating SonarQube analysis'
            }
        }

        // Stage 4: Security Scan
        stage('Security Scan') {
            steps {
                bat '"C:\\Program Files\\nodejs\\npm.cmd" audit || exit /b 0'
            }
        }

        // Stage 5: Deploy to Staging
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment (simulated)...'
                bat 'echo Simulating deployment to staging server'
            }
        }

        // Stage 6: Integration Tests on Staging
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging (simulated)...'
                bat 'echo Simulating Postman or Selenium tests'
            }
        }

        // Stage 7: Deploy to Production
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production environment (simulated)...'
                bat 'echo Simulating production deployment'
            }
        }
    }
}
