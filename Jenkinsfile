pipeline {
    agent any

    // Install the Jenkins tools you need for your project / environment
    tools {
        maven 'maven' // Refers to a global tool configuration for Maven called 'maven-3.8.3'
    }

    // Pull your Snyk token from a Jenkins encrypted credential
    // (type "Secret text"... see https://jenkins.io/doc/book/using/using-credentials/#adding-new-global-credentials)
    // and put it in temporary environment variable for the Snyk CLI to consume.
    environment {
        SNYK_TOKEN = credentials('SNYK_TOKEN')
    }
    
    stages {
    
    stage('Initialize & Cleanup Workspace') {
       echo 'Initialize & Cleanup Workspace'
       sh 'ls -la'
       sh 'rm -rf *'
       sh 'rm -rf .git'
       sh 'rm -rf .gitignore'
       sh 'ls -la'
    }

    stage('Git Clone') {
        git url: 'https://github.com/lmaeda/testproject-java-maven.git'
        sh 'ls -la'
    }

    stage('Test Build Requirements') {
        sh 'java -version'
        sh 'mvn -v'
    }

    // Authorize the Snyk CLI
    withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN_VAR')]) {
        stage('Authorize Snyk CLI') {
            sh './snyk auth ${SNYK_TOKEN_VAR}'
        }
    }

    stage('Build') {
        sh 'mvn package'
    }
    stage('Test') {
      steps {
        echo 'Testing...'
        snykSecurity(
          snykInstallation: 'snyk@latest',
          snykTokenId: 'SNYK_TOKEN',
          failOnIssues: false,
          failOnError: false,
          organization: 'luc.maeda',
          // place other parameters here
        )
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deploying...'
      }
    }
    }
}
