pipeline {
    agent {
        docker {
            image 'node:16' // Use a Node.js Docker image with necessary tools
        }
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm // Checkout your source code from your Git repository
            }
        }
        stage('Build Angular') {
            steps {
                sh 'npm install && npm run build'
            }
        }
        // stage('Maven Build') {
        //     steps {
        //         sh 'mvn' // Build the Java/Maven part of your project
        //     }
        // }
        // stage('Run Tests') {
        //     steps {
        //         sh 'mvn test' // Run tests using Maven
        //     }
        // }
        // stage('Docker Build') {
        //     steps {
        //         script {
        //             docker.build('my-app-image') // Build a Docker image for your app
        //         }
        //     }
        // }
        // stage('Deploy to AWS') {
        //     steps {
        //         // Use AWS CLI or SDK to deploy your application to AWS services (EC2, RDS, etc.)
        //     }
        // }
    }
}
