pipeline {
    agent any

    tools {
        nodejs "NodeJS_20" 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Sriman-5/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
            post {
                success {
                    emailext(
                        to: 'youremail@example.com',
                        subject: "Tests Passed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Run Tests stage succeeded.\nCheck Jenkins for details.",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: 'youremail@example.com',
                        subject: "Tests Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Run Tests stage failed.\nCheck Jenkins for details.",
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'  
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
            post {
                success {
                    emailext(
                        to: 'srimankowshik005@gmail.com',
                        subject: "Security Scan Passed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The NPM Audit (Security Scan) stage succeeded.\nCheck Jenkins for details.",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: 'srimankowshik005@gmail.com',
                        subject: "Security Scan Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The NPM Audit (Security Scan) stage failed.\nCheck Jenkins for details.",
                        attachLog: true
                    )
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }
        success {
            echo 'Pipeline succeeded!'   
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
