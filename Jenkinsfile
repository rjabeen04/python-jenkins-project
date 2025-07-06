pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout your GitHub repo
                git branch: 'main', url: 'https://github.com/rjabeen04/python-jenkins-project.git'
    }
            }
        }

        stage('Setup Python Environment') {
            steps {
                // Create and activate virtual environment, install dependencies
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Lint') {
            steps {
                // Run flake8 linter on my_app folder
                sh '''
                . venv/bin/activate
                flake8 my_app
                '''
            }
        }

        stage('Run Tests with Coverage') {
            steps {
                // Run pytest with coverage, output coverage report in XML format
                sh '''
                . venv/bin/activate
                pytest --cov=my_app --cov-report=xml --junitxml=test-results.xml
                '''
            }
        }

        stage('Publish Coverage Report') {
            steps {
                // Publish Cobertura coverage report from coverage.xml
                cobertura coberturaReportFile: 'coverage.xml'
            }
        }
    }

    post {
        always {
            // Publish JUnit test results and clean workspace after run
            junit 'test-results.xml'
            cleanWs()
        }
    }
}
