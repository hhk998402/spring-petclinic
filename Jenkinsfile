pipeline {
    agent {
        docker { 
            image 'maven',
            args '--net=host'
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
    }
}
