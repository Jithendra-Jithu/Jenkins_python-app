pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/karthikeya964/python-app.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }
        stage('Run Tests') {
            steps {
                sh '''
                . venv/bin/activate
                pytest --junitxml=test-results.xml
                '''
            }
            post {
                always {
                    junit 'test-results.xml'  // Publish test results
                }
            }
        }
        stage('Package Application') {
            steps {
                sh '''
                tar --exclude=venv --exclude=.git --exclude=test-results.xml -czf python-app.tar.gz .
                '''
            }
        }
        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'python-app.tar.gz', fingerprint: true
            }
        }
    }
}
