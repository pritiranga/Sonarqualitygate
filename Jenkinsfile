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

sleep 10
        
stage("Quality Gate") {
    steps {
        script {
            timeout(time: 10, unit: 'MINUTES') {
                def qg = waitForQualityGate()
                if (qg.status != 'OK') {
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                } else {
                    echo 'Quality Gate Passed'
                }
            }
        }
    }
}


    }
}
