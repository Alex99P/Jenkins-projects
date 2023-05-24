def gv
pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages {
        stage('init'){
            steps{
                script {
                    gv = load "script.groovy"
                }
            }
            
        }
        stage('Build jar') {
            steps {
                script {
                    gv.buildJar()
                }
            }
        }
        stage('Build image') {
            steps {
                script {
                    gv.buildImage()
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }
}
