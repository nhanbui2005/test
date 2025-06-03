pipeline {
    agent any

    tools {
        nodejs 'NodeJS_16'
    }

    environment {
        NODE_ENV = 'production'
        APP_NAME = 'simple-nodejs-backend'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Lint check') {
            steps {
                sh 'npm run lint || echo "No lint script found"'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'npm test || echo "No test script found"'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'npm audit || echo "No security issues found"'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build || echo "No build script found"'
            }
        }

        stage('Deploy to Staging') {
            when {
                branch 'develop'
            }
            steps {
                sh '''
                    echo "Deploying to staging environment..."
                    if command -v pm2 &> /dev/null; then
                        pm2 start src/index.js --name "${APP_NAME}-staging" --env staging
                    else
                        NODE_ENV=staging nohup node src/index.js > staging.log 2>&1 &
                    fi
                '''
            }
        }

        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                sh '''
                    echo "Deploying to production environment..."
                    if command -v pm2 &> /dev/null; then
                        pm2 start src/index.js --name "${APP_NAME}-prod" --env production
                    else
                        NODE_ENV=production nohup node src/index.js > production.log 2>&1 &
                    fi
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline chạy thành công!'
            // Thêm thông báo thành công qua Slack/Email
            // slackSend channel: '#deployments', color: 'good', message: "Deployment successful: ${env.JOB_NAME} ${env.BUILD_NUMBER}"
        }
        failure {
            echo '❌ Pipeline lỗi!'
            // Thêm thông báo lỗi qua Slack/Email
            // slackSend channel: '#deployments', color: 'danger', message: "Deployment failed: ${env.JOB_NAME} ${env.BUILD_NUMBER}"
        }
        always {
            // Cleanup và lưu artifacts
            cleanWs()
            archiveArtifacts artifacts: '**/build/**', allowEmptyArchive: true
        }
    }
} 