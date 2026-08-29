pipeline {
    agent any

    options {
        // timestamps()
    }

    stages {
        stage('Checkout from GitHub') {
            steps {
                checkout scm
            }
        }

        stage('Shot repository info') {
            steps {
                sh '''
                    echo "===== GitHub 連携確認 ====="
                    echo "Branch: ${BRANCH_NAME:-unknown}"
                    echo "Build number: ${BUILD_NUMBER}"
                    echo ""
                    echo "===== git remote ====="
                    git remote -v
                    echo ""
                    echo "===== 最新コミット ====="
                    git log -1 --online
                    echo ""
                    echo "===== リポジトリ内のファイル（先頭20件）====="
                    ls -la | head -20
                '''
            }
        }

        stage('Success message') {
            steps {
                echo 'GitHub からの取得に成功しました。'
            }
        }
    }

    post {
        success {
            echo 'OK: GitHub - Jenkins の連携確認が完了しました'
        }
        failure {
            echo 'NG: Checkout または git 操作に失敗しました'
        }
    }
}