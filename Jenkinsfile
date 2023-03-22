def gv
pipeline {
    agent any
    tools{
        maven 'maven-3.6'
    }
    stages {
        stage('init'){
            script {
                gv = load "script.groovy"
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