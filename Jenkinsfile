pipeline {
    agent any
    environment {
        VIRTUAL_ENV = "venv"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/karthikeya964/python-app.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m venv $VIRTUAL_ENV
                . $VIRTUAL_ENV/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }
        stage('Run Tests') {
            steps {
                sh '''
                . $VIRTUAL_ENV/bin/activate
                pytest --junitxml=test-results.xml || true
                '''
            }
            post {
                always {
                    junit 'test-results.xml'
                }
            }
        }
        stage('Package Application') {
            steps {
                sh '''
                tar --exclude=$VIRTUAL_ENV -czf python-app.tar.gz .
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
