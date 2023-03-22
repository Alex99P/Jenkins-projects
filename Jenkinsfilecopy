pipeline {
    agent any
    tools{
        maven 'maven-3.6'
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
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}