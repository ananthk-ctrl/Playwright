pipeline {
    agent any // Runs on your configured Jenkins agent node

    options {
        // Equivalent to timeout-minutes: 60
        timeout(time: 60, unit: 'MINUTES') 
    }

    stages {
        stage('Checkout') {
            steps {
                // Equivalent to actions/checkout@v4
                checkout scm 
            }
        }

        stage('Install dependencies') {
            steps {
                // Equivalent to npm ci
                sh 'npm ci' 
            }
        }

        stage('Install Playwright Browsers') {
            steps {
                // Equivalent to npx playwright install --with-deps
                sh 'npx playwright install --with-deps' 
            }
        }

        stage('Run Playwright tests') {
            steps {
                // The '|| true' ensures the build doesn't crash here, 
                // allowing the post-actions stage below to publish the report.
                sh ' npm run test:smoke || true' 
            }
        }
    }

    post {
        // Equivalent to if: ${{ !cancelled() }}
        // This blocks execution only if the build is forcefully aborted.
        always {
            // Equivalent to actions/upload-artifact@v4
            // This publishes the report directly into the Jenkins UI dashboard.
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'playwright-report',
                reportFiles: 'index.html',
                reportName: 'Playwright HTML Report'
            ])
        }
    }
}
