// Recommendation: Use declarative pipeline syntax for better readability and maintainability
pipeline {
    agent any

    // Recommendation: Parameterize the pipeline for better flexibility
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'The branch to build')
    }

    // Recommendation: Use environment variables for security and ease of use
    environment {
        NEXUS_URL = credentials('NEXUS_URL')
        NEXUS_API_KEY = credentials('NEXUS_API_KEY')
        SONAR_TOKEN = credentials('SONAR_TOKEN')
        SONAR_URL = credentials('SONAR_URL')
        NEXUS_USERNAME = credentials('NEXUS_USERNAME')
        NEXUS_PASSWORD = credentials('NEXUS_PASSWORD')
    }

    stages {
        // Stage 1: Code Checkout
        stage('Code Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: "${params.BRANCH_NAME}"]],
                    userRemoteConfigs: [[url: 'https://github.com/shakilmunavary/AI-Powered-Pipeline-Creation.git', credentialsId: 'GitHubCredentials']]
                ])
            }
        }

        // Stage 2: Build
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        // Stage 3: Unit Testing
        stage('Unit Testing') {
            steps {
                sh 'mvn test'
            }
        }

        // Stage 4: Code Quality Analysis
        stage('Code Quality Analysis') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.projectKey=AI-TEST -Dsonar.host.url=$SONAR_URL -Dsonar.login=$SONAR_TOKEN'
            }
        }

        // Stage 5: Upload Artifacts
        stage('Upload Artifacts') {
            steps {
                sh '''
                    curl -v -u $NEXUS_USERNAME:$NEXUS_PASSWORD --upload-file ./target/AI-Powered-Pipeline-Creation.jar $NEXUS_URL/repository/AI-CI-CD/AI-Powered-Pipeline-Creation.jar
                '''
            }
        }

        // Stage 6: Deployment
        stage('Deployment') {
            steps {
               sh '''
                    echo 'Deployment Complete'
                '''
            }
        }
    }
}
