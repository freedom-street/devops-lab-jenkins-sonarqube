pipeline {
  agent any

  tools {
    maven 'Maven-3.9.6'     // this must match the Maven tool name in Jenkins Global Tool Config
    jdk 'JDK_17'        // must match your JDK tool name in Jenkins
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Test') {
      steps {
        sh 'mvn -f maven-demo/pom.xml clean test'
      }
    }

    stage('SonarQube Scan') {
      steps {
        withSonarQubeEnv('SonarQube') { // must match your Jenkins SonarQube server name
          sh """
            mvn -f maven-demo/pom.xml sonar:sonar \
              -Dsonar.projectKey=devops-lab-jenkins-sonarqube \
              -Dsonar.projectName=devops-lab-jenkins-sonarqube
          """
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
  }
}

