pipeline {
    agent { docker { image 'node:18' } }   // safer: guaranteed Node.js & npm

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Sriman-5/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci || npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Capture test output into test.log
                sh 'npm test > test.log 2>&1 || true'
                archiveArtifacts artifacts: 'test.log', allowEmptyArchive: true
            }
        }

        stage('Generate Coverage Report') {
            steps {
                // Run coverage if defined, don’t fail pipeline
                sh 'npm run coverage > coverage.log 2>&1 || true'
                archiveArtifacts artifacts: 'coverage.log,coverage/**', allowEmptyArchive: true
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                // Save audit output into files
                sh 'npm audit --json > audit.json || true'
                sh 'npm audit > audit.log 2>&1 || true'
                archiveArtifacts artifacts: 'audit.log,audit.json', allowEmptyArchive: true
            }
        }
    }
}
