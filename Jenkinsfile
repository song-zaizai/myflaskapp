pipeline {
    agent any

    environment {
        PYTHON_PATH = 'D:/Program Files/python/python.exe'
    }

    stages {
        stage('Verify Python Path') {
            steps {
                bat """
                    @echo off
                    echo "=== 验证Python313路径及版本 ==="
                    "${PYTHON_PATH}" --version  // <--- 已修改
                    echo "Python路径验证通过！"
                """
            }
        }
        stage('Checkout') {
            steps {
                git url: 'https://github.com/song-zaizai/myflaskapp.git', branch: 'main'
            }
        }
        stage('Fix pip') {
            steps {
                bat """
                    @echo off
                    echo "=== 修复Python313的pip环境 ==="
                    "${PYTHON_PATH}" -m ensurepip --upgrade  // <--- 已修改
                    "${PYTHON_PATH}" -m pip --version       // <--- 已修改
                """
            }
        }
        stage('Install Dependencies') {
            steps {
                bat """
                    @echo off
                    "${PYTHON_PATH}" -m pip install --upgrade pip // <--- 已修改
                    "${PYTHON_PATH}" -m pip install -r requirements.txt // <--- 已修改
                """
            }
        }
        stage('Lint') {
            steps {
                // 注意这里需要将整个命令用双引号包裹，如果命令链中有特殊字符
                bat "\"${PYTHON_PATH}\" -m pip install flake8 && \"${PYTHON_PATH}\" -m flake8 app.py tests/" // <--- 已修改
            }
        }
        stage('Test') {
            steps {
                bat """
                    "${PYTHON_PATH}" -m pip install pytest // <--- 已修改
                    "${PYTHON_PATH}" -m pytest --cov=app tests/ --cov-report=html // <--- 已修改
                """
            }
            post {
                always {
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'htmlcov',
                        reportFiles: 'index.html',
                        reportName: 'Coverage Report'
                    ])
                }
            }
        }
        stage('Build') {
            steps {
                bat """
                    "${PYTHON_PATH}" -m pip install pyinstaller // <--- 已修改
                    "${PYTHON_PATH}" -m PyInstaller --onefile app.py // <--- 已修改
                """
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // 如果是启动应用程序，同样需要引号
                bat "start \"\" \"${PYTHON_PATH}\" app.py"  // <--- 已修改
            }
        }
    }
    post {
        success { echo 'CI/CD pipeline completed successfully!' }
        failure { echo 'CI/CD pipeline failed!' }
    }
}
