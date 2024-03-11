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
        stage("test") {
            steps {
                echo "------- unit test started -----"
                sh 'mvn surefire-report:report'
                echo "------- unit test completed -----"
            }
        }
        
        stage('SonarQube analysis') {
            steps {
                script {
                    withSonarQubeEnv('wilmultd-sonarqube-server') {
                        sh "${env.scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
    }
}
