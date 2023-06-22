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
                withSonarQubeEnv(credentialsId: 'SonarQubeScanner', installationName: 'SonarQubeScanner') {
                    sh './gradlew sonarqube \
                        -Dsonar.projectKey=Sonarqube-QG \
                        -Dsonar.host.url=http://192.168.6.99:9000 \
                        -Dsonar.login=975f2a453ca2eb6e5f4c970238db51e515e4cd87'
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
