pipeline {
  agent any

  tools {
    jdk 'Java17'
    maven 'Maven'
  }

  stages {

    stage('Checkout Code') {
      steps {
        echo 'Pulling from Github'
        git branch: 'main',
            credentialsId: 'gitcred',
            url: 'https://github.com/tanvi712/wipjen.git'
      }
    }

    stage('Test Code') {
      steps {
        echo 'JUNIT Test case execution started'
        dir('indiaproj') {
          bat 'mvn clean test'
        }
      }
      post {
        always {
          junit 'indiaproj/target/surefire-reports/*.xml'
          echo 'Test Run Completed'
        }
      }
    }

    stage('Build Project') {
      steps {
        echo 'Building Java project'
        dir('indiaproj') {
          bat 'mvn clean package -DskipTests'
        }
      }
    }

    stage('Build the Docker Image') {
      steps {
        echo 'Building Docker Image'
        dir('indiaproj') {
          bat 'docker build -t myjavaproj:1.0 .'
        }
      }
    }

    stage('Run Docker Container') {
      steps {
        echo 'Running Java Application'
        bat '''
        docker rm -f myjavaproj-container || exit 0
        docker run --name myjavaproj-container myjavaproj:1.0
        '''
      }
    }
  }

  post {
    success {
      echo 'BUILD and RUN is SUCCESSFUL!'
    }
    failure {
      echo 'OOPS!!! Failure.'
    }
  }
}
