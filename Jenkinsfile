pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Lấy mã nguồn từ repository
                git credentialsId: 'docker', url: 'https://github.com/nguyentruongterabyte/CI_CD.git', branch: 'main'
            }
        }

        stage('Verify Checkout') {
            steps {
                // List directory contents to verify checkout
                bat 'dir'
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
