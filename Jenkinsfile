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
                script {
                    sh 'mvn clean deploy -DskipTests=true'
                }
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
