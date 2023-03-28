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
    // environment {
    //     IMAGE_NAME = 'alexpatroi/demo-app:java-maven-6.0'
    // }

    stages {
        stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }
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
                    def ec2Instance="ec2-user@3.66.236.147"
                    sshagent(['ec2-sever-key']) {
                        sh "scp server-cmd.sh ${ec2Instance}:/home/ec2-user"
                        sh "scp docker-compose.yaml ec2-user@3.66.236.147:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no  ec2-user@3.66.236.147 ${shellCmd}"
                    }
                }
            }
        }
          stage('commit version update ') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'githunconn', variable: 'TOKEN')]) {
                        // git config here for the first time run
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        sh "git remote set-url origin https://$TOKEN@github.com/Alex99P/Jenkins-projects.git"
                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh 'git push origin HEAD:docker-compose'
                    }
                }
            }
        

    }
    }
}