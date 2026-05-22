pipeline {
    agent { label "agent-bhargav" }

    tools {
        jdk 'jdk17'
        maven 'maven3'
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
                dependencyCheck additionalArguments: '--scan ./ --format XML --out target',
                                odcInstallation: 'DP-Check'
            }
        }
        
        stage('Clean & Build') {
            steps {
                sh "mvn clean compile"
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

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Clean & Package') {
            steps {
                sh "mvn clean package -DskipTests"
            }
        }
        
        stage('Docker build and Push'){
            steps{
                script{
                    def imageName = "spring-boot-prof-management"
                    
                    withCredentials([usernamePassword(
                        credentialsId:"DockerHubCred",
                        usernameVariable:"dockerHubUser",
                        passwordVariable:"dockerHubPass")])
                        {
                            sh "docker build -t ${imageName} ."
                            
                            sh "docker tag ${imageName} ${dockerHubUser}/${imageName}:${BUILD_NUMBER}"
                            sh "docker tag ${imageName} ${dockerHubUser}/${imageName}:latest"
                            
                            sh "docker login -u ${dockerHubUser} -p ${dockerHubPass}"
                            
                            sh "docker push ${dockerHubUser}/${imageName}:${BUILD_NUMBER}"
                            sh "docker push ${dockerHubUser}/${imageName}:latest"
                            
                            env.IMAGE_NAME = "${dockerHubUser}/${imageName}:${BUILD_NUMBER}"
                        }
                }
            }
        }
        
        stage('Trivy Image Scan') {
             steps {
                 sh """
                     trivy image \
                     --severity HIGH,CRITICAL \
                     ${env.IMAGE_NAME}
                 """
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
