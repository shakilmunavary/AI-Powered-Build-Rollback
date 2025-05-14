// Recommendation: Use a Jenkinsfile to define your pipeline, it's version-controlled and easier to manage.
// This pipeline uses Groovy syntax, a powerful scripting language for the Java platform.

pipeline {
    agent any

    // Define environment variables
    environment {
        APP_NAME = 'MNC'
        ENV = 'QA'
        REPO_URL = 'https://github.com/shakilmunavary/AI-Powered-Pipeline-Creation'
        NEXUS_URL = credentials('NEXUS_URL')
        NEXUS_API_KEY = credentials('NEXUS_API_KEY')
        SONAR_TOKEN = credentials('SONAR_TOKEN')
        SONAR_URL = credentials('SONAR_URL')
        SONAR_PROJECT_KEY = 'AI-TEST'
    }

    stages {
        // Stage 1: Code Checkout
        stage('Code Checkout') {
            steps {
                git url: REPO_URL
            }
        }

        // Stage 2: Build
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        // Stage 3: Unit Testing
        stage('Unit Testing') {
            steps {
                sh 'mvn test'
            }
        }

        // Stage 4: Code Quality Analysis
        stage('Code quality Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        mvn sonar:sonar \
                          -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                          -Dsonar.host.url=${SONAR_URL} \
                          -Dsonar.login=${SONAR_TOKEN}
                    '''
                }
            }
        }

        // Stage 5: Upload Artifacts
        stage('Upload Artifacts') {
            steps {
                sh 'mvn deploy -DuploadUrl=${NEXUS_URL}/repository/maven-snapshots/ -DrepositoryId=nexus-snapshots'
            }
        }

        // Stage 6: Deployment
        stage('Deployment') {
            steps {
                // Add your deployment steps here
                // For example, you can use the AWS CLI to deploy to an EC2 instance
            }
        }
    }
}