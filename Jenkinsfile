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
                    cp target/*.war /opt/tomcat/webapps/
                    /opt/tomcat/bin/shutdown.sh
                    /opt/tomcat/bin/startup.sh
                    echo "Application Deployed"
                """
            }
        }
    }
}
