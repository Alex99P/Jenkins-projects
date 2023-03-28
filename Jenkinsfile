#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retriver: modernSCM(
    [$class: 'GitSCMSource',
    remote: 'https://github.com/Alex99P/jenkins-shared-library.git',
    credentialsID:'github-credentials']
)

pipeline {
    agent any
    tools {
        maven "maven-3.6"
    }
    environment {
        IMAGE_NAME = 'alexpatroi/demo-app:java-maven-6.0'
    }

    stages {
        stage('Build app') {
            steps {
                script {
                    echo 'building application jar...'
                    buildJar()
                }
            }
        }
        stage('Build image') {
            steps {
                 script {
                    echo 'building docker image...'
                    buildImage(env.IMAGE_NAME)
                    dockerLogin()
                    dockerPush(env.IMAGE_NAME)

                }
            }
        }
        stage('Deploy') {
            steps {
                 script {
                    def shellCmd = "bash ./server-cmd.sh ${IMAGE_NAME}"
                    sshagent(['ec2-sever-key']) {
                        sh "scp server-cmd.sh ec2-user@3.66.236.147:/home/ec2-user"
                        sh "scp docker-compose.yaml ec2-user@3.66.236.147:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no  ec2-user@3.66.236.147 ${shellCmd}"
                    }
                }
            }
        }
    }
}