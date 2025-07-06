pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {

        stage('Checkout') {
            steps {
               git branch: 'main', url: 'https://github.com/rjabeen04/python-jenkins-project.git'

            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                    python3 -m venv $VENV_DIR
                    . $VENV_DIR/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Lint') {
            steps {
                script {
                    def lintStatus = sh(script: '''
                        . $VENV_DIR/bin/activate
                        flake8 my_app
                    ''', returnStatus: true)
                    if (lintStatus != 0) {
                        echo "⚠️ Lint warnings found (flake8), but continuing..."
                    }
                }
            }
        }

        stage('Run Tests with Coverage') {
            steps {
                sh '''
                    . $VENV_DIR/bin/activate
                    pytest --cov=my_app --cov-report=xml --junitxml=tests/test-results/results.xml
                '''
            }
        }

        stage('Publish Coverage Report') {
            steps {
                cobertura coberturaReportFile: 'coverage.xml'
            }
        }

    }

    post {
        always {
            junit 'tests/test-results/results.xml'
        }

        success {
            echo '✅ Build and tests succeeded!'
        }

        failure {
            echo '❌ Build or tests failed.'
        }
    }
}
