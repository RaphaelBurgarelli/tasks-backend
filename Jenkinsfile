pipeline {
    agent any
    stages {
        stage ('Build Backend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Unit Tests') {
            steps {
                bat 'mvn test'
            }
        }
        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                withSonarQubeEnv('SONAR_LOCAL') {
                    bat "\"${scannerHome}\\bin\\sonar-scanner.bat"\" -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=40443a9e3457f9ae3efc99556f367156e333ed3f -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java"
                }
            }
        }
        stage ('Quality Gate') {
            steps {
                sleep(7)
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}