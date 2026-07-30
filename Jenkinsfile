pipeline {
    agent any

    tools {
        nodejs 'NodeJS-18'
    }

    environment {
        SCANNER_HOME = tool 'SonarScanner'
        NODE_OPTIONS = '--max-old-space-size=4096'
    }

    stages {
        stage('Checkout') {
            steps {
                // 'checkout scm' automatically checks out whichever branch triggered the build
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    npm config set fetch-retry-mintimeout 20000
                    npm config set fetch-retry-maxtimeout 120000
                    npm config set fetch-retries 5
                    npm install --legacy-peer-deps --no-audit --no-fund
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh """
                        ${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=react-boilerplate \
                        -Dsonar.sources=. \
                        -Dsonar.exclusions=node_modules/**,build/**
                    """
                }
            }
        }
    }
}


