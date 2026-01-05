pipeline {
    agent any

    parameters {
        string(name: 'GIT_REPO_URL')
        string(name: 'GIT_BRANCH', defaultValue: 'main')
        string(name: 'SONAR_PROJECT_KEY')
        string(name: 'DOCKER_IMAGE_NAME')
        string(name: 'EMAIL_RECIPIENTS')
    }

    environment {
        SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: params.GIT_BRANCH,
                    url: params.GIT_REPO_URL,
                    credentialsId: 'github-creds'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh """
                    ${SONAR_SCANNER_HOME}/bin/sonar-scanner \
                    -Dsonar.projectKey=${params.SONAR_PROJECT_KEY}
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${params.DOCKER_IMAGE_NAME}:${BUILD_NUMBER} ."
            }
        }
    }

    post {
        success {
            emailext(
                subject: "SUCCESS: Build #${BUILD_NUMBER}",
                body: "Docker image ${params.DOCKER_IMAGE_NAME}:${BUILD_NUMBER} built successfully.",
                to: params.EMAIL_RECIPIENTS
            )
        }
        failure {
            emailext(
                subject: "FAILED: Build #${BUILD_NUMBER}",
                body: "Pipeline failed. Please check Jenkins logs.",
                to: params.EMAIL_RECIPIENTS
            )
        }
        aborted {
            emailext(
                subject: "ABORTED: Build #${BUILD_NUMBER}",
                body: "Pipeline aborted. Check Jenkins logs.",
                to: params.EMAIL_RECIPIENTS
            )
        }
    }
}
