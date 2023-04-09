pipeline {
    agent { docker { image 'maven:3.3.3' } }
      stages {
        stage('log version info') {
      steps {
        git 'https://github.com/hhk998402/spring-petclinic.git'
        sh 'mvn --version'
        sh 'mvn clean install'
      }
    }
  }
}
