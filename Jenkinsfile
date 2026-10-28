
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
                    export PATH=/usr/local/bin:$PATH
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
                withCredentials([
                    string(
                        credentialsId: 'snyk-token',
                        variable: 'SNYK_TOKEN'
                    )
                ]) {
                    sh '''
                        export PATH=/usr/local/bin:$PATH
                        venv/bin/python -m pip install -r requirements.txt
                        snyk test --command=venv/bin/python --file=requirements.txt --package-manager=pip
                     '''
                }
            }
        }

        stage('Snyk Monitor') {
            steps {
                withCredentials([
                    string(
                        credentialsId: 'snyk-token',
                        variable: 'SNYK_TOKEN'
                    )
                ]) {
                    sh '''
                        export PATH=/usr/local/bin:$PATH
                        snyk monitor --command=venv/bin/python --file=requirements.txt --package-manager=pip
                     '''
                }
            }
        }

    }
}

