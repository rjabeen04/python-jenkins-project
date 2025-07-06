pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rjabeen04/python-jenkins-project.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Lint') {
            steps {
                sh '''
                    . venv/bin/activate
                    flake8 my_app
                '''
            }
        }

        stage('Run Tests with Coverage') {
            steps {
                sh '''
                    . venv/bin/activate
                    pytest --cov=my_app --cov-report=xml --junitxml=test-results.xml
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
            junit 'test-results.xml'
            cleanWs()
        }
    }
}
