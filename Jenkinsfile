
pipeline {
    agent any
    environment {
        AWS_REGION = 'us-west-2'
        EC2_INSTANCE_ID = 'i-xxxxxxxxxxxxxxxxx'
        NEXUS_URL = 'your_nexus_url'
        SONAR_URL = 'your_sonar_url'
        SONAR_TOKEN = credentials('sonarToken')
        NEXUS_USERNAME = credentials('nexusUsername')
        NEXUS_PASSWORD = credentials('nexusPassword')
        GITHUB_API_KEY = credentials('githubApiKey')
    }
    stages {
        stage('Code Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/shakilmunavary/AI-Powered-Pipeline-Creation'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Unit Testing') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Code Quality Analysis') {
            steps {
                sh "mvn sonar:sonar -Dsonar.projectKey=AI-TEST -Dsonar.host.url=$SONAR_URL -Dsonar.login=$SONAR_TOKEN"
            }
        }
        stage('Upload Artifacts') {
            steps {
                sh "curl -u $NEXUS_USERNAME:$NEXUS_PASSWORD --upload-file target/AI-Powered-Pipeline-Creation.jar $NEXUS_URL/repository/AI-CI-CD/"
            }
        }
        stage('Deployment') {
            steps {
                sh 'aws ec2 start-instances --instance-ids $EC2_INSTANCE_ID --region $AWS_REGION'
                sh 'ssh -o StrictHostKeyChecking=no ec2-user@your_ec2_instance_public_dns scp -o StrictHostKeyChecking=no target/AI-Powered-Pipeline-Creation.jar .'
                sh 'ssh -o StrictHostKeyChecking=no ec2-user@your_ec2_instance_public_dns java -jar AI-Powered-Pipeline-Creation.jar'
                sh 'echo "Successfully deployed"'
            }
        }
    }
}
