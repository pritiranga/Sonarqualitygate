pipeline {
    agent any
    tools{
        gradle 'Gradle'
        }
    stages {
        stage('SonarQube-Code Coverage') {
            environment {
                SCANNER_HOME = tool 'sonartool'
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

        /*stage("Quality gate") {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }*/

          stage("Quality Gate"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"

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
