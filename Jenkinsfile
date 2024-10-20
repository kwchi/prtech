pipeline {
    options { timestamps() }

    agent none
    stages {
        stage('Check scm') {
            agent any
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo "Building...${BUILD_NUMBER}"
                echo "Build completed"
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'python:3.12'
                    args '-u root'
                }
            }  
            steps {
                sh 'pip install --upgrade pip'
                sh 'pip install --no-cache-dir virtualenv'
                sh 'virtualenv venv'
                sh 'venv/bin/pip install --no-cache-dir pytest pytest-cov'
                sh 'venv/bin/python app_test.py'
            }
            post {
                always {
                    echo "Test stage completed"
                }
                success {
                    echo "Application testing successfully completed"
                }
                failure {
                    echo "Oooppss!!! Tests failed!"
                }
            }
        }
    }
}
