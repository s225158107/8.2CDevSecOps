pipeline {
    agent any  // Use the default agent (must be a Windows node for this pipeline)
    
    // Global environment fixes and credentials
    environment {
        // Prepend System32 to PATH to ensure cmd.exe is available (fixes spawn cmd.exe ENOENT):contentReference[oaicite:0]{index=0}
        PATH = "C:\\Windows\\System32;${env.PATH}"
        // (On Windows, %SystemRoot%\system32 is usually in PATH by default. We add it explicitly 
        // here in case a prior PATH override removed it, ensuring 'cmd.exe' can always be found.)
    }
    
    tools {
        // Ensure Node.js is available. If Jenkins NodeJS plugin is installed, use it:
        // nodejs "MyNodeJS" 
        // (Alternatively, make sure Node is installed on the agent and in PATH. 
        // Jenkins will use the Node from PATH if not using a tool installation.)
    }
    
    stages {
        stage('Install Dependencies') {
            steps {
                // Use 'bat' for Windows commands (instead of 'sh'):contentReference[oaicite:1]{index=1}
                bat 'npm install' 
                // (This runs the same as "npm install" in a Windows cmd shell.)
            }
        }
        
        stage('Build') {
            steps {
                // Example build step (if applicable)
                bat 'npm run build --if-present' 
                // (Use --if-present to avoid error if a build script isn't defined, similar to coverage below.)
            }
        }
        
        stage('Test') {
            steps {
                // Run tests; ensure to use bat on Windows
                bat 'npm test'
            }
        }
        
        stage('Coverage') {
            steps {
                // Run coverage script if it exists, without failing if it doesn't exist.
                // The --if-present flag prevents npm from erroring out if 'coverage' script is not defined:contentReference[oaicite:2]{index=2}.
                bat 'npm run coverage --if-present'
                
                // (If no "coverage" script is in package.json, this will simply do nothing and not error. 
                // If the coverage script exists, it will run. You can add an echo or logging here if you want to indicate coverage was skipped or run.)
            }
        }
        
        stage('SonarCloud Analysis') {
            environment {
                // Inject SonarCloud token from Jenkins credentials (assumes a Secret Text credential with ID 'SONARCLOUD_TOKEN')
                SONAR_TOKEN = credentials('SONARCLOUD_TOKEN')
            }
            steps {
                // Run SonarCloud scanner using npx in a batch step. Using %SONAR_TOKEN% to pass the token securely.
                bat """
                    npx sonar-scanner ^
                      -Dsonar.projectKey=YOUR_PROJECT_KEY ^          /* Sonar project details */
                      -Dsonar.organization=YOUR_ORG ^                /* SonarCloud organization */
                      -Dsonar.host.url=https://sonarcloud.io ^       /* SonarCloud URL */
                      -Dsonar.login=%SONAR_TOKEN%                    /* Authentication token */
                """
                // The multi-line string (^) is used for readability; replace projectKey/organization with your values.
                // Using %SONAR_TOKEN% ensures the token from Jenkins credentials is used (masked in logs). 
                // Note: Ensure Java is installed on the agent since sonar-scanner requires Java.
            }
        }
        
        stage('Snyk Security Scan') {
            environment {
                // Inject Snyk API token from Jenkins credentials (create a Secret Text credential with ID 'SNYK_TOKEN'):contentReference[oaicite:3]{index=3}
                SNYK_TOKEN = credentials('SNYK_TOKEN')
            }
            steps {
                // **Authenticate with Snyk** – ensure the Snyk CLI is logged in before testing.
                bat "snyk auth %SNYK_TOKEN%"
                // (This uses the token from Jenkins credentials to authenticate Snyk CLI in a non-interactive, secure way.)
                
                // Run Snyk vulnerability test
                bat 'snyk test'
                // (Make sure the Snyk CLI is installed on the agent, e.g., via npm or chocolatey, and on the PATH. 
                // If not, you may install it in an earlier step or use the Snyk Jenkins plugin as an alternative.)
            }
        }
    }
}
