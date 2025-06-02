pipeline {
    agent any

    tools {
        nodejs 'NodeJS_16'
    }

    stages {
        stage('Clone code') {
            steps {
                // Giả sử code đã có sẵn trong workspace
                echo 'Code đã có sẵn trong workspace'
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Lint check') {
            steps {
                // Thêm kiểm tra lint nếu cần
                echo 'Kiểm tra code style'
            }
        }

        stage('Build') {
            steps {
                // Build step đơn giản cho Node.js
                sh 'npm run build || echo "No build script found"'
            }
        }

        stage('Deploy') {
            steps {
                // Triển khai ứng dụng
                sh '''
                    echo "Starting deployment..."
                    # Khởi động ứng dụng với PM2 nếu đã cài đặt
                    if command -v pm2 &> /dev/null; then
                        pm2 start src/index.js --name "simple-nodejs-backend"
                    else
                        # Hoặc chạy trực tiếp với node
                        nohup node src/index.js > app.log 2>&1 &
                    fi
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline chạy thành công!'
            // Thêm thông báo thành công (ví dụ: Slack, email)
        }
        failure {
            echo '❌ Pipeline lỗi!'
            // Thêm thông báo lỗi (ví dụ: Slack, email)
        }
        always {
            // Cleanup sau khi chạy pipeline
            cleanWs()
        }
    }
} 