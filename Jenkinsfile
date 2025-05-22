pipeline {
    agent any

    environment {
        NODE_ENV = 'test'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/saitejaswini72/8.1CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage'
            }
        }

        stage('NPM Audit') {
            steps {
                sh 'npm audit'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        echo "Running SonarCloud analysis..."

                        curl -O https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
                        unzip -o sonar-scanner-cli-5.0.1.3006-linux.zip
                        cd sonar-scanner-5.0.1.3006-linux/bin
                        export PATH=$PATH:$(pwd)
                        cd ../../../
                        ./sonar-scanner-5.0.1.3006-linux/bin/sonar-scanner -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
    }
}
