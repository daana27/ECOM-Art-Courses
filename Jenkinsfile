#!/usr/bin/env groovy

node {
    tools {
        'org.jenkinsci.plugins.docker.commons.tools.DockerTool' '18.09'
    }
    environment {
    DOCKER_CERT_PATH = credentials('docker')
    }
    stages{
        stage('foo') {
          steps {
            sh "docker version" // DOCKER_CERT_PATH is automatically picked up by the Docker client
          }
        }
        stage('checkout') {
            checkout scm
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

}
