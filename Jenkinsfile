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
            
            // Explicitly defining mail properties to bypass global Jenkins config issues
            emailext(
                subject: "Playwright Test Report - Build #${env.BUILD_NUMBER}",
                body: "Tests finished with status: ${currentBuild.currentResult}.\nCheck report here: ${env.BUILD_URL}",
                to: 'shelkenikita186@gmail.com',
                attachmentsPattern: 'playwright-report/**/*',
                mailType: '',
                // Forces it to use direct SMTP settings
                replyTo: '',
                from: 'shelkenikita186@gmail.com'
            )
        }
    }
}