pipeline {
    agent any

    environment {
        SONAR_SCANNER_HOME = tool 'Sonar-Scan'
    }

    stages {

        stage('Clone from GitHub') {
            steps {
                git url: 'https://github.com/jeinnn4/Sonarqube5.git', branch: 'main'
                echo 'Cloning Done'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('Jenkin-To-Sonarqube') {
                    sh """
                        ${SONAR_SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=jenkin \
                        -Dsonar.projectName=jenkin \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://34.205.41.107:9000 \
                        -Dsonar.login=$SONAR_AUTH_TOKEN
                    """
                }
                echo 'Scanning Done'
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
