pipeline {
    agent any

    environment {

        // SonarQube
        SONAR_HOST_URL = 'http://172.17.0.1:9000'
        SONAR_PROJECT_KEY = 'demo-vscode'

        // Nexus
        NEXUS_URL = 'http://172.17.0.1:8081'
        NEXUS_REPO = 'maven-releases'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'your_password'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Code pulled from GitHub'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Creating build file'
                sh 'echo "Hello DevOps Pipeline" > output.txt'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    script {
                        def scannerHome = tool 'sonar-scanner'

                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=${SONAR_HOST_URL}
                        """
                    }
                }
            }
        }

        stage('Package') {
            steps {
                sh 'zip -r app-${BUILD_NUMBER}.zip output.txt'
            }
        }

        stage('Upload to Nexus') {
            steps {
                sh """
                    curl -u ${NEXUS_USER}:${NEXUS_PASS} \
                    --upload-file app-${BUILD_NUMBER}.zip \
                    ${NEXUS_URL}/repository/${NEXUS_REPO}/app-${BUILD_NUMBER}.zip
                """
            }
        }

        stage('Finish') {
            steps {
                echo "Pipeline Completed Successfully"
            }
        }
    }
}