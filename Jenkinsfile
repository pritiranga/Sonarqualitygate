pipeline {
    agent any
    tools{
        gradle 'Gradle'
        }
    stages {
        stage('SonarQube-Code Coverage') {
            environment {
                SCANNER_HOME = tool 'SonarQubeScanner'
            }
            steps {
                withSonarQubeEnv(credentialsId: 'sonarcred', installationName: 'SonarQubeScanner') {
                    sh 'chmod +x gradlew'
                    sh './gradlew sonarqube \
                        -Dsonar.projectKey=test \
                        -Dsonar.host.url=http://192.168.6.99:9000 \
                        -Dsonar.login=66deb091946f0ca486b6aef5e1992ea3ed2eca77'
                } 
            } 
        } 

            stage("Quality Gate") {
                steps {
                    timeout(time: 1, unit: 'HOURS') {
                        script {
                            def analysis = waitForQualityGate()
                            def sonarTaskId = analysis.getId()
                            def sonarURL ='http://192.168.6.99:9000'

                            // Construct the URL to access the SonarQube task
                            def taskURL = "${sonarURL}/api/ce/task?id=${sonarTaskId}"
                            echo "SonarQube task URL: ${taskURL}"
                        }
                    }
                }
            }


        /*stage('Build and Test') {
            steps {
                script {
                    // Run your unit tests here
                    junit(testResults: 'build/test-results/test/*.xml', allowEmptyResults: true, skipPublishingChecks: true)
                    def testResult = sh(returnStatus: true, script: './gradlew test')
                    if (testResult != 0) {
                        error('Unit tests failed. Failing the build.')
                    }
                }
            }
        } */
    }
}
