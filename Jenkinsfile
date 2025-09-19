pipeline {
  agent any

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
        sh 'npm test || true | tee test.log'
      }
      post {
        always {
          emailext(
            subject: "Jenkins: Run Tests - ${currentBuild.currentResult}",
            body: """Hello Sriman,

The Run Tests stage has finished with status: ${currentBuild.currentResult}.

You can also check Jenkins here: ${BUILD_URL}

Logs are attached.
""",
            to: "srimankowshik005@gmail.com",
            attachmentsPattern: 'test.log'
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
        sh 'npm audit || true | tee npm-audit.log'
      }
      post {
        always {
          emailext(
            subject: "Jenkins: Security Scan - ${currentBuild.currentResult}",
            body: """Hello Sriman,

The Security Scan stage has finished with status: ${currentBuild.currentResult}.

You can also check Jenkins here: ${BUILD_URL}

Logs are attached.
""",
            to: "srimankowshik005@gmail.com",
            attachmentsPattern: 'npm-audit.log'
          )
        }
      }
    }
  }
}
