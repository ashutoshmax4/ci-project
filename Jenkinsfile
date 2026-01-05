pipeline {
    agent any

    parameters {
        string(name: 'SONAR_PROJECT_KEY', defaultValue: 'ci-project')
        string(name: 'DOCKER_IMAGE', defaultValue: 'ci-project-image')
        string(name: 'EMAIL_RECIPIENTS', defaultValue: 'yourmail@gmail.com')
    }

    environment {
        SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    ${SONAR_SCANNER_HOME}/bin/sonar-scanner \
                      -Dsonar.projectKey=${SONAR_PROJECT_KEY}
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} .
                '''
            }
        }
    }

    post {
        success {
            emailext(
                subject: "SUCCESS: Build #${BUILD_NUMBER}",
                body: "Docker image built successfully.",
                to: params.EMAIL_RECIPIENTS
            )
        }
        failure {
            emailext(
                subject: "FAILED: Build #${BUILD_NUMBER}",
                body: "Build failed. Check Jenkins logs.",
                to: params.EMAIL_RECIPIENTS
            )
        }
    }
}

