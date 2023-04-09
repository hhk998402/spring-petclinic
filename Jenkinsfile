pipeline {
    agent {
        docker { 
            image 'hemanthhk/maven-with-jq:latest'
            args '--network assignment-1_devops-assignment-1'
        }
    }
    stages {
        stage('Clone repository') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: 'main']],
                    userRemoteConfigs: [[url: 'https://github.com/hhk998402/spring-petclinic.git']]
                ])
            }
        }
        stage('Build and test') {
            steps {
                sh 'mvn clean package'
                sh 'mvn test'
            }
        }
        stage('SonarQube') {
            steps {
                script {
                    def sonarProj = 'spring-petclinic'
                    def sonarURL = 'http://sonarqube:9000'
                    def sonarToken = 'sqa_6b2060cf4106ba48f8025a25276c45aa450757fb'

                    // Check if SonarQube project exists
                    def sonarProjectExists = sh(returnStdout: true, script: "curl -s -u ${sonarToken}: ${sonarURL}/api/projects/search | jq -r '.components[] | select(.key == \"${sonarProj}\")'")
                    if (sonarProjectExists) {
                        // Submit test report for analysis
                        sh "mvn sonar:sonar -Dsonar.host.url=${sonarURL} -Dsonar.login=${sonarToken}"
                    } else {
                        // Create new SonarQube project
                        sh "mvn sonar:sonar -Dsonar.host.url=${sonarURL} -Dsonar.login=${sonarToken} -Dsonar.projectKey=${sonarProj} -Dsonar.projectName=${sonarProj}"
                    }
                }
            }
        }
    }
}
