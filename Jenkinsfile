#!/usr/bin/env groovy

node {
    stage('checkout') {
        checkout scm
    }
    
    tools {
        // Install the Maven version configured as "Default" and add it to the path.
         maven "Default"
         nodejs "Default"
         docker "Default"
    }
    docker.image('jhipster/jhipster:v8.0.0-beta.2').inside('-u jhipster -e MAVEN_OPTS="-Duser.home=./"') {
        stage('check java') {
            sh "java -version"
        }

        stage('clean') {
            sh "chmod +x mvnw"
            sh "./mvnw -ntp clean -P-webapp"
        }
        stage('nohttp') {
            sh "./mvnw -ntp checkstyle:check"
        }

        stage('install tools') {
            sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:install-node-and-npm@install-node-and-npm"
        }

        stage('npm install') {
            sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:npm"
        }
        stage('backend tests') {
            try {
                sh "./mvnw -ntp verify -P-webapp"
            } catch(err) {
                throw err
            } finally {
                junit '**/target/surefire-reports/TEST-*.xml,**/target/failsafe-reports/TEST-*.xml'
            }
        }

        stage('frontend tests') {
            try {
               sh "npm install"
               sh "npm test"
            } catch(err) {
                throw err
            } finally {
                junit '**/target/test-results/TESTS-results-jest.xml'
            }
        }

        stage('packaging') {
            sh "./mvnw -ntp verify -P-webapp -Pprod -DskipTests"
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
        }
    }
}

// pipeline {
//     agent any
    
//     tools {
//         // Install the Maven version configured as "Default" and add it to the path.
//         maven "Default"
//         nodejs "Default"
//     }

//     stages {
//          // Get some code from a GitHub repository
//         stage('checkout'){
//             steps{
//                 git url : 'https://github.com/deschcla/ECOM-Art-Courses.git', branch:"ci-cd"
//             }
//         }
//         stage('Build frontend'){
//             steps{
//                 sh "npm install"
//             }
//         }
//         stage('check java') {
//             steps{
//                 sh "java -version"   
//             }
            
//         }
//         stage('clean') {
//             steps{
//                 sh "chmod +x mvnw"
//                 sh "./mvnw -ntp clean -P-webapp"
//             }
//         }
//         stage('nohttp') {
//             steps{
//                 sh "./mvnw -ntp checkstyle:check"
                
//             }
//         }

//         stage('packaging') {
//             steps{
//                 sh "./mvnw -ntp verify -P-webapp -Pprod -DskipTests"
//                 archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                    
//             }
//          }
//     }
// }
