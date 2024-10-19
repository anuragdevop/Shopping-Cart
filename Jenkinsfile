pipeline {
    agent any
    
    tools {
        jdk 'jdk11' // Specify JDK version
        maven 'maven3' // Specify Maven version
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner' // Path to the SonarQube scanner
        SONAR_REPORT_PATH = 'target/sonar/report' // Adjust as needed
        EMAIL_RECIPIENTS = 'your-email@example.com' // Specify the recipient's email
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                script {
                    echo "Checking out code from GitHub..."
                    git branch: 'main', changelog: false, credentialsId: 'your-credentials-id', poll: false, url: 'https://github.com/jaiswaladi246/Shopping-Cart.git'
                }
            }
        }
        
        stage('Build') {
            steps {
                script {
                    echo "Building the project..."
                    sh "mvn clean package -DskipTests=true"
                }
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    echo "Running SonarQube analysis..."
                    withSonarQubeEnv('sonar-server') {
                        sh '''
                            $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=Shopping-Cart \
                            -Dsonar.projectName=Shopping-Cart \
                            -Dsonar.java.binaries=target/classes \
                            -Dsonar.sourceEncoding=UTF-8
                        '''
                    }
                }
            }
        }
        
        stage('Generate Sonar Report') {
            steps {
                script {
                    echo "Generating SonarQube report..."
                    sh "curl -u 'your-sonarqube-credentials' 'http://your-sonarqube-server/api/measures/component_tree?component=Shopping-Cart&metricKeys=sqale_index,vulnerabilities,bugs,code_smells' -o sonar-report.json"
                }
            }
        }
        
        stage('Email Report') {
            steps {
                script {
                    echo "Sending report via email..."
                    emailext (
                        subject: "SonarQube Report for Shopping-Cart",
                        body: """
                        Please find the attached SonarQube report for the Shopping-Cart project.
                        
                        Report Details:
                        ${readFile('sonar-report.json')}
                        """,
                        to: "${EMAIL_RECIPIENTS}",
                        attachLog: true
                    )
                }
            }
        }
    }
}
