#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage('foo') {
            steps {
                script {
                    def dockerTool = tool name: 'DockerTool', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
                    withEnv(["PATH+DOCKER=${dockerTool}/bin"]) {
                        sh "docker version"
                    }
                }
            }
        }
        stage('checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build and Test') {
            steps {
                script {
                    def jhipsterImage = docker.image('jhipster/jhipster:v8.0.0-beta.2').inside('-u jhipster -e MAVEN_OPTS="-Duser.home=./"') {
                        sh "java -version"
                        sh "chmod +x mvnw"
                        sh "./mvnw -ntp clean -P-webapp"
                        sh "./mvnw -ntp checkstyle:check"
                        sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:install-node-and-npm@install-node-and-npm"
                        sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:npm"

                        try {
                            sh "./mvnw -ntp verify -P-webapp"
                        } catch (err) {
                            throw err
                        } finally {
                            junit '**/target/surefire-reports/TEST-*.xml,**/target/failsafe-reports/TEST-*.xml'
                        }

                        sh "npm install"
                        try {
                            sh "npm test"
                        } catch (err) {
                            throw err
                        } finally {
                            junit '**/target/test-results/TESTS-results-jest.xml'
                        }

                        sh "./mvnw -ntp verify -P-webapp -Pprod -DskipTests"
                        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                    }
                }
            }
        }
    }
}
