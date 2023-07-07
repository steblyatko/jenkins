// Declarative //
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'make'
                archiveArtifacts artifacts: "**/target/*.jar", fingerprint: true
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'make check || true'
                junit '**/target/*.xml'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                when {
                  expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                  }
                }
                steps {
                  sh 'make publish'
                }
            }
        }
    }
}
// Script //
node {
  checkout scm

  stage('Build') {
    echo 'Building....'
    sh 'make'
    archiveArtifacts artifacts: "**/target/*.jar", fingerprint: true

  }
  stage('Test') {
    echo 'Building....'
    sh 'make check || true'
    junit '**/target/*.xml'
  }
  stage('Deploy') {
    echo 'Deploying....'
    if (currentBuild.result == null || currentBuild.result == 'SUCCESS') {
      sh 'make publish'
    }
    steps {
      sh 'make publish'
    }
  }
}