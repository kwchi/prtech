pipeline {
    options{timestamps()}

    agent none
    stages {
        stage('Check scm'){
            agent any
            steps {
                checkout scm
            }
        }
        stage('Build'){
            steps{
                echo "Building...${BUILD_NUMBER}"
                echo "Build completed"
            }
        }
        stage('Test'){
            agent { docker { image 'python:3.12' args '-u root' } }  
            steps {
                sh 'pip install --no-cache-dir pytest pytest-cov'
                sh 'python -m venv venv'
                sh 'venv/bin/python -m pip install pytest pytest-cov'  
                sh 'venv/bin/python app_test.py' 
            }
            post {
                always {
                    junit '**/test_result.xml'
                }
                success{
                    echo "Application testing successfully completed"
                }
                failure{
                    echo "Oooppss!!! Tests failed!"
                }
            }
        }
    }
}