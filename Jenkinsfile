pipeline {
    agent {
        node {
            label 'maven'
        }
    }
    
    environment {
        PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
        scannerHome = tool 'wilmultd-sonar-scanner'
    }
    
    stages {
        stage("Build") {
            steps {
                echo "------- build started -----"
                sh 'mvn clean deploy -DskipTests=true'
                echo "------- build completed -----"
            }
        }
        
        stage("Test") {
            steps {
                echo "------- unit test started -----"
                sh 'mvn test'
                echo "------- unit test completed -----"
            }
        }
        
        stage('Generate Surefire Report') {
            steps {
                echo "------- surefire report generation started -----"
                sh 'mvn surefire-report:report'
                echo "------- surefire report generation completed -----"
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                echo "------- SonarQube analysis started -----"
                withSonarQubeEnv('wilmultd-sonarqube-server') {
                    sh "${env.scannerHome}/bin/sonar-scanner"
                }
                echo "------- SonarQube analysis completed -----"
            }
        }
    }
}
