pipeline {
    agent {
        docker { 
            image 'hemanthhk/maven-with-jq:latest'
            args '--network assignment-2_devops-assignment-1'
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
                withCredentials([usernamePassword(credentialsId: 'sonarqubeToken', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                    script {
                        def sonarProj = 'spring-petclinic'
                        def sonarURL = 'http://sonarqube:9000'
                        def sonarToken = "${PASSWORD}"

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
        stage('Run using Docker') {
            agent any
            steps {
                sh 'ansible-playbook --inventory dev.inv ansible-playbook.yml"
            }
        }
    }
}
