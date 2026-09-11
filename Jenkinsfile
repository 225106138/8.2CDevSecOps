pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/225106138/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }
        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
        }
        stage('SonarCloud Analysis') {
            steps {
                sh '''
                    # Download the SonarScanner CLI (only if not already present)
                    if [ ! -d sonar-scanner-6.2.1.4610-linux-x64 ]; then
                        apt-get update && apt-get install -y unzip
                        curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-6.2.1.4610-linux-x64.zip
                        unzip -o sonar-scanner.zip
                    fi

                    # Run the scan; token comes from the SONAR_TOKEN env var
                    export PATH="$PATH:$(pwd)/sonar-scanner-6.2.1.4610-linux-x64/bin"
                    sonar-scanner -Dsonar.token=$SONAR_TOKEN
                '''
            }
        }
    }
}
