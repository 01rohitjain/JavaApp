pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'GITHUB_CRED', url: 'https://github.com/01rohitjain/JavaApp.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn install -U -P jar -DskipTests'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                sh '''
                mvn clean verify sonar:sonar -Dsonar.host.url=http://<IP>:9000 -Dsonar.login=<SonarQubeToken> -Dsonar.qualitygate.wait=true
                '''
            }
        }
        stage('Code Coverage') {
            steps {
                jacoco maximumBranchCoverage: '30', maximumComplexityCoverage: '40', maximumInstructionCoverage: '2', maximumLineCoverage: '40', sourcePattern: '**/src/main/java/**'
            }
        }
                stage('Security Scan') {
            steps {
                dependencyCheck additionalArguments: ''' 
                    -o './'
                    -s './'
                    -f 'ALL' 
                    --prettyPrint''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
            }
        }
    }
    post {
        always {
            dependencyCheckPublisher pattern: 'dependency-check-report.xml'
        }
        success {
            emailext subject: "Build Success", body: "The build was successful", to: 'ro****@gmail.com'
        }
        failure {
            emailext subject: "Build Failure", body: "The build failed", to: 'ro****@gmail.com'
        }
    }
}

