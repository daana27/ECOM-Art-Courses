pipeline {
    agent any
    
    tools {
        // Install the Maven version configured as "Default" and add it to the path.
        maven "Default"
        nodejs "Default"
    }

    stages {
         // Get some code from a GitHub repository
        stage('checkout'){
            steps{
                git url : 'https://github.com/deschcla/ECOM-Art-Courses.git', branch:"ci-cd"
            }
        }
        stage('Build frontend'){
            steps{
                sh "npm install"
            }
        }
        stage('check java') {
            steps{
                sh "java -version"   
            }
            
        }
        stage('clean') {
            steps{
                sh "chmod +x mvnw"
                sh "./mvnw -ntp clean -P-webapp"
            }
        }
        stage('nohttp') {
            steps{
                sh "./mvnw -ntp checkstyle:check"
                
            }
        }

        stage('packaging') {
            steps{
                sh "./mvnw -ntp verify -P-webapp -Pprod -DskipTests"
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                    
            }
        }
    }
}
