#!/usr/bin/env groovy

@Library('jenkins-shared-library') //we put ' _ ', if don't have other variables after Library
def gv

pipeline {
    agent any
    tools{
        maven 'maven-3.6'
    }

    stages {
        stage('init'){
            steps {
                script{
                    gv = load "script.groovy"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo "Testing the application.."
                    echo "Executing pipeline for branch $BRANCH_NAME"
                }
            }
        }
        stage('Build jar') {
            steps {
                 script {
                    buildJar()

                }
            }
        }
        stage('Build image') {
            steps {
                 script {
                   buildImage 'alexpatroi/my-jenkins:jma-3.3'
                   dockerLogin()
                   dockerPush 'alexpatroi/my-jenkins:jma-3.3'
                }
            }
        }
    }
}