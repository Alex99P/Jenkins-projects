#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retriver: modernSCM(
    [$class: 'GitSCMSource',
    remote: 'https://github.com/Alex99P/jenkins-shared-library.git',
    credentialsID:'github-credentials']
)

pipeline {
    agent {
         docker {
            image 'docker:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    tools {
        maven "maven-3.6"
    }
    environment {
        IMAGE_NAME = 'alexpatroi/demo-app:java-maven-1.0'
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
        stage('Build') {
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
                    def dockerComposeCmd = 'docker-compose -f docker-compose.yaml up --detach'
                    sshagent(['ec2-sever-key']) {
                        sh "scp docker-compose.yaml ec2-user@3.66.236.147:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no  ec2-user@3.66.236.147 ${dockerComposeCmd}"
                    }
                }
            }
        }
    }
}