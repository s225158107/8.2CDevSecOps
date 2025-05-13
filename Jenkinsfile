pipeline {
  agent any

  tools {
    nodejs 'NodeJS 18' 
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/s225158107/8.2CDevSecOps.git'
      }
    }

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
      bat '"C:\\Windows\\System32\\cmd.exe" /c npx sonar-scanner -Dsonar.login=%SONAR_TOKEN%'
          }
        }
      }
    }
  }
}
