pipeline {
    agent { label "agent-bhargav" }

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    environment {
        SONAR_SERVER = 'SonarQube-Server'
    }

    stages {

        stage('Code checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/munnavuyyuru/Spring-app-DevSecOps-CICD.git'
            }
        }

        stage('OWASP Dependency-Check') {
            steps {
                dependencyCheck additionalArguments: '--scan "./"',
                                odcInstallation: 'DP-Check'
            }
        }

        stage('Sonarqube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube-Server') {
                    withCredentials([string(credentialsId: 'sonar', variable: 'SONAR_TOKEN')]) {
                        sh """
                            mvn sonar:sonar \
                            -Dsonar.host.url=http://65.1.192.163:9000 \
                            -Dsonar.login=$SONAR_TOKEN \
                            -Dsonar.java.binaries=target/classes
                        """
                    }
                }
            }
        }

        stage('Clean & Package') {
            steps {
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Run the app') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }

    post {
        always {
            dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        }
    }
}
