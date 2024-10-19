pipeline {
    agent any
    tools {
        jdk 'jdk11'
        maven 'maven3'
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: 'your-credentials-id', url: 'https://github.com/anuragdevop/Shopping-Cart.git'
            }
        }

        stage('Compile Code') {
            steps {
                sh "mvn clean compile -DskipTests=true"
            }
        }

        // Add other stages as necessary
    }

    post {
        success {
            script {
                emailext(
                    subject: "Successful Build: ${currentBuild.fullDisplayName}",
                    body: "The build was successful!\nCheck the details at: ${env.BUILD_URL}",
                    to: 'anurag.suraj23@gmail.com'
                )
            }
        }
        failure {
            script {
                emailext(
                    subject: "Failed Build: ${currentBuild.fullDisplayName}",
                    body: "The build has failed!\nCheck the details at: ${env.BUILD_URL}",
                    to: 'anurag.suraj23@gmail.com'
                )
            }
        }
    }
}
