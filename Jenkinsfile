pipeline {
    agent any

    environment {
        SONAR_HOST = 'http://172.17.0.1:9000'
        SONAR_PROJECT = 'demo-vscode'

        NEXUS_URL = 'http://172.17.0.1:8081'
        NEXUS_REPO = 'raw-repo'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'your_password'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Code pulled from GitHub'
            }
        }

        stage('Build') {
            steps {
                echo 'Preparing artifact'
                sh 'echo "Hello DevOps Pipeline" > output.txt'
            }
        }

        stage('SonarQube') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''
                        echo "Simulating SonarQube scan"
                        sleep 2
                    '''
                }
            }
        }

        stage('Package') {
            steps {
                sh 'zip -r app.zip output.txt'
            }
        }

        stage('Upload to Nexus') {
            steps {
                sh """
                    curl -u ${NEXUS_USER}:${NEXUS_PASS} \
                    --upload-file app.zip \
                    ${NEXUS_URL}/repository/${NEXUS_REPO}/app-${BUILD_NUMBER}.zip
                """
            }
        }

        stage('Finish') {
            steps {
                echo 'Pipeline completed successfully'
            }
        }
    }
}