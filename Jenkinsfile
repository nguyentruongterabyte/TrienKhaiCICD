pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Lấy mã nguồn từ repository
                git credentialsId: 'docker', url: 'https://github.com/nguyentruongterabyte/TrienKhaiCICD.git', branch: 'main'
            }
        }

        stage('Verify Checkout') {
            steps {
                // Liệt kê những đường dẫn cần check out
                bat 'dir'
                // Kiểm tra file docker-compose.yml có tồn tại không
                script {
                    if (!fileExists('docker-compose.yml')) {
                        error "docker-compose.yml file not found"
                    }
                }
            }
        }

        stage('Build and Deploy') {
            steps {
                script {
                    // Xây dựng và triển khai bằng Docker Compose
                    try {
                        bat 'docker-compose down'
                        // Chỉ khởi động lại dịch vụ WordPress
                        bat 'docker-compose up -d'
                    } catch (Exception e) {
                        echo "Error during Docker Compose operations: ${e}"
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }

        stage('Post-deploy Cleanup') {
            steps {
                // Bất kỳ bước dọn dẹp nào sau triển khai, nếu cần
                bat 'docker system prune -f'
            }
        }
    }

    post {
        always {
            // Luôn luôn lưu trữ log
            archiveArtifacts artifacts: '**/logs/**', allowEmptyArchive: true
        }
    }
}
