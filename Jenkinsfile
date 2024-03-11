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
                script {
                    sh 'mvn clean deploy -Dmaven.test.skip=true'
                }
                echo "------- build completed -----"
            }
        }

        stage("SonarQube analysis") {
            steps {
                script {
                    withSonarQubeEnv('wilmultd-sonarqube-server') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
    }
}
