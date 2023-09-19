#!/usr/bin/env groovy
node {
    stage('checkout') {
        checkout scm
    }
    withCredentials([usernameColonPassword(credentialsId: '6aa2882d-fb9f-4995-985e-5e737302ca68', variable: 'DOCKERHUB')]) {
        sh '''
          docker login -u $DOCKERHUB_USR -p $DOCKERHUB_PSW
        '''
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
        
        // stage('backend tests') {
        //     try {
        //         sh "./mvnw -ntp verify -P-webapp"
        //     } catch(err) {
        //         throw err
        //     } finally {
        //         junit '**/target/surefire-reports/TEST-*.xml,**/target/failsafe-reports/TEST-*.xml'
        //     }
        // }

        // stage('frontend tests') {
        //     try {
        //        sh "npm install"
        //        sh "npm test"
        //     } catch(err) {
        //         throw err
        //     } finally {
        //         junit '**/target/test-results/TESTS-results-jest.xml'
        //     }
        // }

        // stage('packaging') {
        //     sh "./mvnw -ntp verify -P-webapp -Pprod -DskipTests"
        //     archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
        // }
        
        // stage('Build Docker Image') {  
        //     steps{                     
        //     	sh 'docker build -t rocdaana27/ecom-art-courses:0.1 .'     
        //     	echo 'Build Image Completed'                
        //     }           
        // } 
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
