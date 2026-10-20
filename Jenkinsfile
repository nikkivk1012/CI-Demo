pipeline {
    agent any

    tools {
        nodejs 'Node'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                bat 'npm ci'
            }
        }

        stage('Install Playwright Browsers') {
            steps {
                bat 'npx playwright install --with-deps'
            }
        }

        stage('Run Playwright Tests') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    bat 'npx playwright test'
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'playwright-report/**/*', allowEmptyArchive: true
            
            emailext(
                subject: "Playwright Test Report - Build #${env.BUILD_NUMBER} (${env.JOB_NAME})",
                body: """Hi,

The automated Playwright test suite has completed.
- Build Status: ${currentBuild.currentResult}
- Console Output & Report: ${env.BUILD_URL}

Regards,
Jenkins CI
""",
                to: 'shelkenikita186@gmail.com',
                attachmentsPattern: 'playwright-report/**/*'
            )
        }
    }
}