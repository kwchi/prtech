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
            agent{docker {image 'alpine'
                args '-u="root"'
                }
            }
            steps {
                sh 'apk add --update python3 py-pip'
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate'
                sh 'pip install xmlrunner'
                sh 'python3 app_test.py'
            }
            post {
                always {
                    junit 'test-reports/*.xml'
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