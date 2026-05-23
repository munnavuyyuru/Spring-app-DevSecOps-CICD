# Spring-app-DevSecOps-CICD

A DevSecOps CI/CD pipeline project for a Spring Boot application using Jenkins, SonarQube, OWASP Dependency-Check, Trivy, and Docker.


##  Tech Stack

*   **Language**: Java 17
*   **Framework**: Spring Boot
*   **Build Tool**: Maven
*   **Orchestration**: Docker & Docker Compose
*   **CI/CD Engine**: Jenkins
*   **Security & Quality Tools**: SonarQube, OWASP Dependency-Check, Trivy

##  Project Structure

```text
Spring-app-DevSecOps-CICD/
├── .mvn/
│   └── wrapper/           
├── src/                    
├── .gitignore             
├── Dockerfile              
├── Jenkinsfile             
├── docker-compose.yml      
├── mvnw                    
├── mvnw.cmd                
└── pom.xml                 
```

## ⚙️ CI/CD Architecture
<img width="1193" height="502" alt="image" src="https://github.com/user-attachments/assets/8060763d-c877-4c32-8e9a-c7750f6dc673" />

##  Jenkins Pipeline

The Jenkins pipeline automates the complete DevSecOps workflow:

1.  Source Code Checkout
2.  Dependency Vulnerability Scan
3.  Maven Build
4.  SonarQube Code Analysis
5.  Quality Gate Validation
6.  Docker Image Build
7.  Docker Hub Push
8.  Trivy Image Scan
9.  Application Deployment

### 📸 Pipeline Success
<img width="1057" height="552" alt="Screenshot 2026-05-22 125347" src="https://github.com/user-attachments/assets/d1aa0e71-4b2c-4032-86c9-65d76cae675f" />

##  Local Setup

### 1. Clone Repository
```bash
git clone https://github.com/munnavuyyuru/Spring-app-DevSecOps-CICD.git
cd Spring-app-DevSecOps-CICD
```

### 2. Build Application
```bash
./mvnw clean package
```

### 3. Run Application
```bash
docker compose up -d
```

##  Jenkins Requirements

### Required Tools
*   JDK 17
*   Maven 3
*   Docker
*   Trivy
*   SonarQube
*   OWASP Dependency-Check

### Required Jenkins Plugins
*   Pipeline
*   Git
*   SonarQube Scanner
*   OWASP Dependency-Check
*   Docker Pipeline
*   Credentials Binding

### 🔑 Jenkins Credentials


| Credential ID | Purpose |
| :--- | :--- |
| `sonar` | SonarQube Token |
| `DockerHubCred` | Docker Hub Credentials |

##  Pipeline Stages Breakdown


| Stage | Description |
| :--- | :--- |
| **Code Checkout** | Pull source code from GitHub |
| **Dependency Check** | Scan dependencies for vulnerabilities |
| **Build** | Compile Spring Boot application |
| **SonarQube Analysis** | Perform static code analysis |
| **Quality Gate** | Validate code quality |
| **Docker Build** | Build Docker image |
| **Docker Push** | Push image to Docker Hub |
| **Trivy Scan** | Scan Docker image vulnerabilities |
| **Deploy** | Run application using Docker Compose |


##  License
This project is licensed under the MIT License.


