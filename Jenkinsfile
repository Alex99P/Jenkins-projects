pipeline {
    agent any
    tools{
        maven 'Maven'
    }
    stages {
        stage('Build jar') {
            steps {
                script {
                echo 'Building the application'
                sh 'mvn package'
                }
            }
        }
        stage('Build image') {
            steps {
                script {
                echo 'Building the docker image'
                withCredentials([usernamePassword(credentialsId:'docker-hub-repo', passwordVariable: 'PASS',usernameVariable:'USER' )]){
                    sh 'docker build -t alexpatroi/my-jenkins:jma-2.0 .'
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push alexpatroi/my-jenkins:jma-2.0'
                }

                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}