
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/jaswanthBavi/devsecops-cicd-pipeline.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 --version
                    python3 -m venv venv
                    venv/bin/python -m pip install --upgrade pip
                    venv/bin/python -m pip install -r requirements.txt
                '''
            }
        }

        stage('Unit Tests') {
            steps {
                sh '''
                    venv/bin/python -m unittest app.test_app -v
                '''
            }
        }

        stage('Snyk Security Scan') {
            steps {
                sh 'snyk test'
            }
        }

        stage('Snyk Monitor') {
            steps {
                sh 'snyk monitor'
            }
        }
    }
}

