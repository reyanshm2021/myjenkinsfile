pipeline {
    agent any

    environment {
        SONAR_HOME = tool "Sonar"
    }

    stages {
        stage("Code Clone from GitHub") {
            steps {
                echo "Cloning repository from GitHub..."
                git url: "https://github.com/mohitlavania07/my-dir.git", branch: "main"
            }
        }

        stage("SonarQube Quality Analysis") {
            steps {
                echo "Running SonarQube analysis..."
                withSonarQubeEnv("Sonar") {
                    sh """
                        $SONAR_HOME/bin/sonar-scanner \
                        -Dsonar.projectName=wanderlust \
                        -Dsonar.projectKey=wanderlust \
                        -Dsonar.sources=.
                    """
                }
            }
        }

        stage("OWASP Dependency Check") {
            steps {
                echo "Running OWASP Dependency Check..."
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'dc'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }

        stage("Sonar Quality Gates Scan") {
            steps {
                echo "Checking SonarQube quality gate results..."
                timeout(time: 2, unit: "MINUTES") {
                    waitForQualityGate abortPipeline: false
                }
            }
        }

        stage("Trivy File System Scan") {
            steps {
                echo "Running Trivy file system scan..."
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        stage(Deploy){
            steps{
                sh "docker-compose up -d --build "
            }
        }
    }
}
