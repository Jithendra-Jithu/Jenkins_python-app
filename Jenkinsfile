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
                source venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }
        stage('Run Tests') {
            steps {
                sh '''
                source venv/bin/activate
                pytest --junitxml=test-results.xml
                '''
            }
        }
        stage('Package Application') {
            steps {
                sh '''
                tar -czf python-app.tar.gz .
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
