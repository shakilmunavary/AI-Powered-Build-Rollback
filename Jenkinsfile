pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/shakilmunavary/AI-Powered-Build-Rollback.git' // Update with your repo
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean package"
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    sudo cp target/*.war /opt/tomcat/webapps/
                    sudo /opt/tomcat/bin/shutdown.sh
                    sudo /opt/tomcat/bin/startup.sh
                    echo "Application Deployed"
                """
            }
        }
    }
}
