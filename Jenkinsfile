node {

    def mavenHome = tool 'maven' // Refers to a global tool configuration for Maven called 'maven-3.8.4'
    env.PATH="${env.PATH}:$mavenHome/bin"

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

}
