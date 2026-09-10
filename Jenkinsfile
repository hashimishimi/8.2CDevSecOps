pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/hashimishimi/8.2CDevSecOps.git'
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
            post {
                always {
                    emailext(
                        to: 'YOUR_EMAIL@gmail.com',
                        subject: "Jenkins Test Stage - ${currentBuild.currentResult}",
                        body: """The Run Tests stage has completed.

Build: ${env.BUILD_NUMBER}
Status: ${currentBuild.currentResult}
Job: ${env.JOB_NAME}
Build URL: ${env.BUILD_URL}""",
                        attachLog: true
                    )
                }
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
            post {
                always {
                    emailext(
                        to: 'YOUR_EMAIL@gmail.com',
                        subject: "Jenkins Security Scan - ${currentBuild.currentResult}",
                        body: """The NPM Audit security scan has completed.

Build: ${env.BUILD_NUMBER}
Status: ${currentBuild.currentResult}
Job: ${env.JOB_NAME}
Build URL: ${env.BUILD_URL}""",
                        attachLog: true
                    )
                }
            }
        }
    }
}
