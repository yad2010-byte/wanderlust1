pipeline {
    agent any
    environment {
        SONAR_HOME=tool "Sonar"
    }
    stages{
        stage("Clone Code from GitHub "){
            steps{
                git url: "https://github.com/yad2010-byte/wanderlust1.git",branch:"main"
            }
        }
        stage("SonarQube Quality Analysis"){
            steps{
                withSonarQubeEnv("Sonar"){
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=wanderlust1 -Dsonar.projectKey=wanderlust1"
                }
            }
        }
        stage("OWASP Dependancy Check"){
            steps{
                 withCredentials([string(credentialsId: 'NVD_API_KEY', variable: 'NVD_KEY')]) {
                     //additionalArguments: "--nvdApiKey ${NVD_KEY} -s . -f ALL"
                    dependencyCheck additionalArguments: '--nvdApiKey ${NVD_KEY} -s . -f ALL --scan ./', odcInstallation: 'dc-owasp'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml' 
                 }
                
            }
        }
        stage("Sonar QualityGate Scan"){
            steps{
                timeout(time: 2, unit: "MINUTES"){
                    waitForQualityGate abortPipeline: false 
                }
            }
        }
        stage("Trivy File System Scan"){
            steps{
                sh "trivy fs --format table -o trivy-fs-report.html ." 
            }
        }
        stage("Deploy using docker compose"){
            steps{
                sh "docker compose up -d --build"
            }
            
        }
    }
}
