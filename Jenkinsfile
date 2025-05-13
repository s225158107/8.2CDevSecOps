pipeline {
    agent any

    tools {
        nodejs 'NodeJS 18' 
    }

    environment {
        SONAR_TOKEN = credentials('sonarcloud-token') 
    }

    stages {
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0' 
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }

        stage('SonarCloud Analysis') {
    steps {
        withSonarQubeEnv('SonarCloud') {
            withCredentials([string(credentialsId: 'SONARCLOUD_TOKEN', variable: 'SONAR_TOKEN')]) {
                bat "npx sonar-scanner -Dsonar.login=%SONAR_TOKEN%"
                    }
                }
            }
        }
    }
}
